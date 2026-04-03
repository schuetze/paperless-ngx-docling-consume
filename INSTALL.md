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
C:\Users\user>podman machine list
NAME                     VM TYPE     CREATED         LAST UP     CPUS        MEMORY      DISK SIZE
podman-machine-default*  wsl         55 seconds ago  Never       16          16GiB       100GiB

C:\Users\user>
````



