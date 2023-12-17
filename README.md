# Jellyfin Docker Swarm stack that allows for fault tollerant [rffmpeg](https://github.com/joshuaboniface/rffmpeg).

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
- Directory /transcodes exists
    ```
  mkdir /transcodes
    
- SSD with enough space to handle transcodes
- Docker swarm is already setup and running
- Nodes within the swarm that meet all assumptions have a label 'transcode' with a value of '1'
