# Jellyfin Docker Swarm stack that allows for fault tollerant [rffmpeg](https://github.com/joshuaboniface/rffmpeg).

Host System Assumptions:
- SSD with enough space to handle transcodes
- Docker swarm is setup and running
- Intel QSV CPU
- Debian or Ubuntu
- Intel specific setup is followed including fixing all limitations of various kernels
    ```
    https://jellyfin.org/docs/general/administration/hardware-acceleration/intel/#verify-on-linux
    ```
    ```
    https://jellyfin.org/docs/general/administration/hardware-acceleration/known-issues
- Low Power Mode is setup
    ```
    https://jellyfin.org/docs/general/administration/hardware-acceleration/intel#lp-mode-hardware-support
- OpenCL is installed
  ```
  sudo apt install intel-opencl-icd
- NFS/NFSD is installed
    ```
    sudo apt install nfs-common
    sudo apt install nfs-server
    sudo sh -c "echo 'nfsd' >> /etc/modules"
    sudo sh -c "echo 'nfs' >> /etc/modules"
    sudo modprobe nfsd nfs
- Apparmor is removed
    ```
    sudo systemctl stop apparmor.service
    sudo systemctl disable apparmor.service
    sudo apt purge apparmor
    sudo sed -i 's/GRUB_CMDLINE_LINUX_DEFAULT="[^"]*"/GRUB_CMDLINE_LINUX_DEFAULT="apparmor=0"/' /etc/default/grub
    sudo update-grub
    
- Directory /transcodes and /cache exists with group ownership to users
    ```
  sudo mkdir /transcodes
  sudo chgrp users /transcodes
  sudo mkdir /cache
  sudo chgrp users /cache
  sudo chmod 775 /transcodes
  sudo chmod 775 /cache
    

- Nodes are running a local container that allows for device mapping within swarm
  ```
  Original:
  docker run -i --restart always -d --name device-manager --privileged --cgroupns=host --pid=host --userns=host -v /sys:/host/sys -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/gitdeath/device-mapping-manager:master
  ```



