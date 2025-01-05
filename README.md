# Jellyfin Docker Swarm stack that allows for fault tollerant [rffmpeg](https://github.com/joshuaboniface/rffmpeg).
      NOTE: This isn't fully functioning at this time - Do not use
Host System Assumptions:
- Intel QSV CPU
- Debian or Ubuntu
- Intel specific setup is followed including fixing all limitations of various kernels
    ```
    https://jellyfin.org/docs/general/administration/hardware-acceleration/intel#linux-setups
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
- Directory /transcodes and /cache exists with group ownership to users
    ```
  sudo mkdir /transcodes
  sudo chgrp users /transcodes
  sudo mkdir /cache
  sudo chgrp users /cache 
- SSD with enough space to handle transcodes
- Docker swarm is already setup and running
- Nodes are running a local container that allows for device mapping within swarm
  ```
  Original:
  docker run -i --restart always -d --name device-manager --privileged --cgroupns=host --pid=host --userns=host -v /sys:/host/sys -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/allfro/allfro/device-mapping-manager:latest

  Note: the below includes a fix that will map devices for containers already running.
  docker run -i --restart always -d --name device-manager --privileged --cgroupns=host --pid=host --userns=host -v /sys:/host/sys -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/gitdeath/device-mapping-manager:master
  ```
- Nodes within the swarm that meet all assumptions have a label 'transcode' with a value of '1'





# Testing
dmesg | grep -iE "huc|guc|dmc"

```
[    1.425611] Setting dangerous option enable_guc - tainting kernel
[    1.478689] i915 0000:00:02.0: firmware: direct-loading firmware i915/kbl_dmc_ver1_04.bin
[    1.479020] i915 0000:00:02.0: [drm] Finished loading DMC firmware i915/kbl_dmc_ver1_04.bin (v1.4)
[    1.479478] i915 0000:00:02.0: firmware: direct-loading firmware i915/kbl_guc_70.1.1.bin
[    1.479582] i915 0000:00:02.0: firmware: direct-loading firmware i915/kbl_huc_4.0.0.bin
[    1.483515] i915 0000:00:02.0: [drm] GuC firmware i915/kbl_guc_70.1.1.bin version 70.1.1
[    1.483521] i915 0000:00:02.0: [drm] HuC firmware i915/kbl_huc_4.0.0.bin version 4.0.0
[    1.507042] i915 0000:00:02.0: [drm] HuC authenticated
[    1.507051] i915 0000:00:02.0: [drm] GuC submission disabled
[    1.507055] i915 0000:00:02.0: [drm] GuC SLPC disabled
```
