# Jellyfin Docker Swarm stack that allows for fault tollerant [rffmpeg](https://github.com/joshuaboniface/rffmpeg).
```NOTE: This isn't functioning at this time - Do not use```
Host System Assumptions:
- Intel QSV CPU
- Debian or Ubuntu
- Low Power Mode is setup
    ```
    https://jellyfin.org/docs/general/administration/hardware-acceleration/intel#lp-mode-hardware-support
- NFSD is installed
    ```
    apt install nfs-server
    sh -c "echo 'nfsd' >> /etc/modules"
    modprobe nfsd
- Apparmor is removed
    ```
    sudo systemctl stop apparmor.service
    sudo systemctl disable apparmor.service
    sudo apt purge apparmor
- Directory /transcodes exists with group ownership to users
    ```
  mkdir /transcodes
  chgrp users /transcodes  
- SSD with enough space to handle transcodes
- Docker swarm is already setup and running
- Nodes are running a local container that allows for device mapping within swarm
  ```
  docker run -i --rm -d --name device-manager --privileged --cgroupns=host --pid=host --userns=host -v /sys:/host/sys -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/allfro/allfro/device-mapping-manager:latest
  ```
- Nodes within the swarm that meet all assumptions have a label 'transcode' with a value of '1'
