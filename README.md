# Ubuntu VM Boilerplate

A simple bash script to handle boilerplate configurations for cloned Ubuntu VMs (Machine ID, SSH server keys, Hostname)

## Instructions for Increasing LVM size to match the Ubuntu VM's allocated space

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
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
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
