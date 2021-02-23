# Ubuntu VM Boilerplate

A simple bash script to handle boilerplate configurations for cloned Ubuntu VMs (Machine ID, SSH server keys, Hostname)

## Expand LVM
```sh
sudo parted /dev/sda
resizepart 3 100%
quit
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
