# qvm
Quick/QEMU VM launcher
<br><br>

#### Usage:

```
$ qvm

  qvm <configfile> [-qemuopt1 -qemuopt2 ...]

```

Requires a configuration file for an argument and optionally takes multiple options to pass directly to `qemu`.

<br><br>

```
$ qvm foo

/home/you/foo: configuration file not found.
Create it now from template? [y/N] y

############################################################
## qvm configuration
##
## Tips:
##
## NET= <boolean> | bridge0 | br0 | virbr0
## MAC= <blank> | 52:54:01:23:45:67
##   (blank/empty means random)
## ICH9= <boolean>
##   (host chipset Q35/ICH9 or i440FX/PIIX3)
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

#NAME="Enterprise Linux 7"
#ISO="Springdale_Linux-7.9-x86_64-netinst.iso"
#IMG="springdale.qcow2"
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

Saved in: /home/you/foo
Edit this file and set IMG= and/or ISO=
```

Launches the virtual machine using the configuration file `foo` if available, otherwise creates it.

<br><br>

```
$ vim foo
ISO="PeppermintOS-amd64.iso"
```
```
$ qvm foo

Configuration file: /home/you/foo
IMG= disk image file not set.

VM name    [foo]
boot from  [PeppermintOS-amd64.iso]
NIC MAC    [52:54:50:0e:39:ce]
```

Boots from the specified `.iso` image. (Good for testing but not suitable for installation without a disk image attached.)

<br><br>

```
$ vim foo
IMG="pepper.qcow2"
```
```
$ qvm foo

Configuration file: /home/you/foo

IMG=pepper.qcow2: inaccessible or not found.
Create empty disk image file now? [y/N] y
Disk image size: [50G] 
File format: [qcow2] 
Name: [pepper.qcow2] 

Formatting 'pepper.qcow2', fmt=qcow2 cluster_size=65536 extended_l2=off compression_type=zlib size=53687091200 lazy_refcounts=off refcount_bits=16
```

Launches the VM using the disk image `pepper.qcow2` if available, otherwise asks to create it.
