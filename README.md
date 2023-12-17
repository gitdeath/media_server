Host System Assumptions:
- Intel QSV CPU
- Debian or Ubuntu
- Low Power Mode is setup
    ```https://jellyfin.org/docs/general/administration/hardware-acceleration/intel#lp-mode-hardware-support```
- NFSD is installed
    ```
    apt install nfs-server
    sh -c "echo 'nfsd' >> /etc/modules"
    modprobe nfsd```
- Apparmor is removed
    ```sudo systemctl stop apparmor.service
    sudo systemctl disable apparmor.service
    sudo apt purge apparmor```
- Directory /transcodes exists
    ```mkdir /transcodes```
