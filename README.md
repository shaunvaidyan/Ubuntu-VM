# Ubuntu VM Boilerplate

A simple bash script to handle boilerplate configurations for cloned Ubuntu VMs (Machine ID, SSH server keys, Hostname)

## Add Disk Space to VM
**1. Add Hard Disk in Hardware tab of VM in Proxmox GUI**
```
Use SCSI Bus and Check the Discard Option (Trims when using thin provisioning)
```
```
The aim is to provision the new disk as an LVM physical volume (PV), extend the existing volume group (VG) to span both the new and old disk, and then expand the logical volume (LV)
```
**2. Find the VG and LV names**
```
sudo vgs
sudo lvs
```

**3. Run commands**
```
sudo pvcreate /dev/sdb
sudo vgextend ubuntu-vg /dev/sdb
sudo lvextend ubuntu-vg/ubuntu-lv -l+100%FREE
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
reboot
```

## Instructions for Increasing LVM size to match the Ubuntu VM's already allocated space

**1. Resize the Partition**
```sh
sudo parted /dev/sda
resizepart 3 100%
quit
# reload partition table
partprobe
```
**2. Expand the LVM physical volume**
```sh
sudo pvresize /dev/sda3
```

**3. Expand the logical volume**
```sh
sudo lvresize -l +100%FREE /dev/mapper/ubuntu-vg/ubuntu-lv
```

**4. Expand the filesystem**
```sh
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```
## Usage

Run the following command from a bash session, you will be prompted for a new hostname, and whether you wish to reboot the system.

**NOTE: Make sure you wait for all services to start before running this script, otherwise weirdness may ensue!**

```sh
sudo bash -c "bash <(curl -f -L -sS https://raw.githubusercontent.com/shaunvaidyan/Ubuntu-VM/master/boilerplate/run.sh)"
```

```sh
sudo touch /etc/cloud/cloud-init.disabled
```
