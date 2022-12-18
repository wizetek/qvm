# qvm
Quick/QEMU VM launcher
<br><br><br>
#### Usage:
```
$ qvm myconfigfile
```
Use settings in `myconfigfile` to launch VM. Create a new one on first use or if not found. 
<br><br><br>
```
$ qvm pepper.qvm
$ vim pepper.qvm
ISO="PeppermintOS-amd64.iso"
IMG=pepper.qcow2

$ qvm pepper.qvm
```
Use disk image `pepper.qcow2` if available, otherwise prompt to create one. Boot from `.iso` image first.
<br><br><br>
#### Configuration:
```
############################################################
## qvm configuration
##
## Tips:
##
## NET= <boolean> | bridge0 | br0 | virbr0
## MAC= <blank> | 52:54:01:23:45:67
##   (blank/empty means random)
## ICH9= <boolean>
##   (host chipset ICH9 or I440FX)
##
## OPT=
## -hdb /path/to/seconddisk.img
## -hdd fourthdisk.img
##   (-hdc conflicts with -cdrom)
## -drive format=raw,media=cdrom,readonly,file=cd.iso
## -nic model=virtio-net-pci
## -usb -device usb-tablet
## -display gtk,gl=on,window-close=off
## -device VGA,edid=on,xres=1366,yres=768
## -daemonize
## ...and other qemu options.
############################################################

NAME="Peppermint"
ISO="PeppermintOS-amd64.iso"
IMG="pepper.qcow2"
RAW="no"
ICH9="no"
CPU="2"
MEM="4G"
NET="yes"
#MAC="52:54:01:23:45:67"
#VNC=":2"
#TELNET="2302"
AUDIO="no"
#OPT="-display gtk"
```
