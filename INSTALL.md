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
### install some basic stuff to work with, I guess you only really need git :)
````bash
sudo dnf install tmux wget btop git -y
````
Expected output:
````
[user@DESKTOP-RR74GPE ~]$ sudo dnf install tmux wget btop git -y
Updating and loading repositories:
 Fedora 43 - x86_64 - Updates                                                                                                                                                          100% |   2.9 MiB/s |  10.7 MiB |  00m04s
 Copr repo for containers-podman-28250 owned by packit                                                                                                                                 100% |   2.8 KiB/s |   7.8 KiB |  00m03s
 Fedora 43 openh264 (From Cisco) - x86_64                                                                                                                                              100% |   1.6 KiB/s |   5.8 KiB |  00m04s
 Fedora 43 - x86_64                                                                                                                                                                    100% |   6.1 MiB/s |  35.4 MiB |  00m06s
Repositories loaded.
Package                                                                     Arch             Version                                                                      Repository                                       Size
Upgrading:
 ngtcp2                                                                     x86_64           1.21.0-1.fc43                                                                updates                                     318.3 KiB
   replacing ngtcp2                                                         x86_64           1.19.0-1.fc43                                                                updates                                     314.2 KiB
 ngtcp2-crypto-gnutls                                                       x86_64           1.21.0-1.fc43                                                                updates                                      39.6 KiB
   replacing ngtcp2-crypto-gnutls                                           x86_64           1.19.0-1.fc43                                                                updates                                      39.6 KiB
Installing:
 btop                                                                       x86_64           1.4.6-1.fc43                                                                 updates                                       1.7 MiB
 git                                                                        x86_64           2.53.0-1.fc43                                                                updates                                      56.4 KiB
 tmux                                                                       x86_64           3.5a-7.fc43                                                                  updates                                       1.2 MiB
 wget2-wget                                                                 x86_64           2.2.1-1.fc43                                                                 updates                                      42.0   B
Installing dependencies:
[...]
````




