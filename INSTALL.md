# Gettting this stuff up and running with rootless podman containers

## we are on windows and use wsl

### Check that podman for windows is ready
````bash
podman machine list
````
Expected output:
````
C:\Users\user>podman machine list
NAME        VM TYPE     CREATED     LAST UP     CPUS        MEMORY      DISK SIZE
````
### Create a brand new instance of the podman machine
Lets have a little extra RAM, you never know ;)
````bash
podman machine init -m 16384
````
Expected output:
````
C:\Users\user>podman machine init -m 16384
Looking up Podman Machine image at quay.io/podman/machine-os:5.8 to create VM
Getting image source signatures
Copying blob 78121c715464 done   |
Copying config 44136fa355 done   |
Writing manifest to image destination
78121c715464005a887ec92c1a929b35d8f157ab6e3a030547a2f4a38c013610
Extracting compressed file: podman-machine-default-amd64: done
Importing operating system into WSL (this may take a few minutes on a new WSL install)...
The operation completed successfully.
vConfiguring system...
Machine init complete
To start your machine run:

        podman machine start


C:\Users\user>
````
### check again
````bash
podman machine list
````
Expected output:
````
C:\Users\user>podman machine list
NAME                     VM TYPE     CREATED         LAST UP     CPUS        MEMORY      DISK SIZE
podman-machine-default*  wsl         55 seconds ago  Never       16          16GiB       100GiB

C:\Users\user>
````
### start the machine
````bash
podman machine start
````
Expected output:
````
C:\Users\user>podman machine start
Starting machine "podman-machine-default"

This machine is currently configured in rootless mode. If your containers
require root permissions (e.g. ports < 1024), or if you run into compatibility
issues with non-podman clients, you can switch using the following command:

        podman machine set --rootful

API forwarding listening on: npipe:////./pipe/docker_engine

Docker API clients default to this address. You do not need to set DOCKER_HOST.
Machine "podman-machine-default" started successfully

C:\Users\user>
````
### connect to the machine
````bash
podman machine ssh
````
Expected output:
````
C:\Users\user>podman machine ssh
Connecting to vm podman-machine-default. To close connection, use `~.` or `exit`
[user@DESKTOP-RR74GPE ~]$
````
Yuearh, we are in, lets do this
### install some basic stuff to work with ( I guess you only really need podman-compose and git :) )
````bash
sudo dnf install tmux wget btop podman-compose git -y
````
Expected output, basically a wall of text:
````
[user@DESKTOP-RR74GPE ~]$ sudo dnf install tmux wget btop podman-compose git -y
Updating and loading repositories:
Repositories loaded.
Package "wget2-wget-2.2.1-1.fc43.x86_64" is already installed.

Package                                                                                                                             Arch                       Version                                                                                                                              Repository                                                                        Size
Installing:
 btop                                                                                                                               x86_64                     1.4.6-1.fc43                                                                                                                         updates                                                                        1.7 MiB
 git                                                                                                                                x86_64                     2.53.0-1.fc43                                                                                                                        updates                                                                       56.4 KiB
 podman-compose                                                                                                                     noarch                     1.5.0-4.fc43                                                                                                                         fedora                                                                       667.4 KiB
 tmux                                                                                                                               x86_64                     3.5a-7.fc43                                                                                                                          updates                                                                        1.2 MiB
Installing dependencies:
[...]
````

### install the GPU stuff for podman in preperation for docling
````bash
curl -s -L https://nvidia.github.io/libnvidia-container/stable/rpm/nvidia-container-toolkit.repo |  sudo tee /etc/yum.repos.d/nvidia-container-toolkit.repo 
sudo yum install -y nvidia-container-toolkit 
sudo nvidia-ctk cdi generate --output=/etc/cdi/nvidia.yaml
nvidia-ctk cdi list
 
podman run --rm --device nvidia.com/gpu=all nvidia/cuda:11.0.3-base-ubuntu20.04 nvidia-smi

````
Expected output:
````
````

### clone the repos of paperless-ngx and the paperless-ngx-docling-consume 
````bash
cd
git clone https://github.com/paperless-ngx/paperless-ngx
git clone https://github.com/BoxcarFields/paperless-ngx-docling-consume.git
````
Expected output:
````
[user@DESKTOP-RR74GPE ~]$ cd
git clone https://github.com/paperless-ngx/paperless-ngx
git clone https://github.com/BoxcarFields/paperless-ngx-docling-consume.git
Cloning into 'paperless-ngx'...
remote: Enumerating objects: 95674, done.
remote: Counting objects: 100% (1156/1156), done.
remote: Compressing objects: 100% (455/455), done.
remote: Total 95674 (delta 952), reused 710 (delta 701), pack-reused 94518 (from 4)
Receiving objects: 100% (95674/95674), 182.21 MiB | 17.14 MiB/s, done.
Resolving deltas: 100% (72266/72266), done.
Cloning into 'paperless-ngx-docling-consume'...
remote: Enumerating objects: 20, done.
remote: Counting objects: 100% (20/20), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 20 (delta 5), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (20/20), 12.21 KiB | 1.53 MiB/s, done.
Resolving deltas: 100% (5/5), done.
````
### prepare all the files into your "working directory", I am putting everything into the paperless-ngx directory here
````bash
cd
cd paperless-ngx
cp docker/compose/docker-compose.postgres-tika.yml .
cp docker/compose/docker-compose.env .
cp ../paperless-ngx-docling-consume/docling-postconsume.sh .
chmod +x docling-postconsume.sh
````
Expected output:
````
[user@DESKTOP-RR74GPE paperless-ngx]$ cd
cd paperless-ngx
cp docker/compose/docker-compose.postgres-tika.yml .
cp docker/compose/docker-compose.env .
cp ../paperless-ngx-docling-consume/docling-postconsume.sh .
chmod +x docling-postconsume.sh
[user@DESKTOP-RR74GPE paperless-ngx]$
````

### edit the files 
````bash

````
Expected output:
````

````
### start the whole thing to get the API ley
````bash
podman compose up -d
````
Expected output:
````
well, it should start
````
### connect, create the admin user, log in, extract api key from gui user pane
````bash
http://localhost:8000/dashboard
````
top right corner, user, my profile, API Auth Token

<img width="1396" height="668" alt="image" src="https://github.com/user-attachments/assets/70b6ac5f-1f01-4ef5-a318-a527d4a299b8" />



