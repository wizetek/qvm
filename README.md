# qvm
Quick/QEMU VM launcher

Bash shell script to quickly create QEMU virtual machines and disk images with separate configuration for each.
<br>

## Installation

```
$ chmod +x qvm
```

<br>

## Usage

```
qvm <configfile> [-qemuopt1 -qemuopt2 ...]
```

Requires a configuration file as an argument and optionally takes multiple options to pass directly to `qemu`.

<br>

## Examples

#### 1.
```
$ qvm foo
/home/you/.config/qvm/foo: Configuration file not found
Create it now from template? y/(n)
############################################################
## qvm configuration
##
## Tips:
##
## NET= <boolean> | bridge0 | br0 | virbr0
##   (empty means enabled)
## MAC= <empty> | 52:54:01:23:45:67
##   (empty means random)
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
#
#NAME="Linux distro"
#ISO="linux.iso"
#IMG="disk.qcow2"
#RAW="yes"
CPU="2"
MEM="4G"
#ICH9="yes"
#AUDIO="yes"
#NET="no"
#MAC="52:54:01:23:45:67"
#VNC=":2"
#TELNET="2302"
#OPT="-usb -device usb-tablet"

Saved in: /home/you/.config/qvm/foo
Edit this file and set IMG= and/or ISO=
```

Attempts to launch the virtual machine using the configuration file `foo` if available, otherwise only creates it.

<br>

#### 2.
```
/home/you/.config/qvm/foo
...
...
ISO="PeppermintOS-amd64.iso"
...
...
```

```
$ qvm foo
Configuration file: /home/you/.config/qvm/foo

VM name     [foo]
boot from   [PeppermintOS-amd64.iso]
MAC address [52:54:50:0e:39:ce]
```

Boots from the `.iso` image set in the configuration file. (Good for testing but not suitable for installation without a disk image attached – see below.)

<br>

#### 3.
```
/home/you/.config/qvm/foo
...
...
ISO="PeppermintOS-amd64.iso"
IMG="peppermint.qcow2"
...
...
```

```
$ qvm foo
Configuration file: /home/you/.config/qvm/foo
IMG=peppermint.qcow2: Inaccessible or not found
Prepare disk image? or quit (y)/n/q 
Disk image size (50G) : 
File format (qcow2)/raw/vdi/vhdx/vmdk : 
Name (peppermint.qcow2) : 
Create now? or redo/quit (y)/n/q 
Formatting 'peppermint.qcow2', fmt=qcow2 cluster_size=65536 extended_l2=off compression_type=zlib size=53687091200 lazy_refcounts=off refcount_bits=16

VM name     [foo]
boot from   [PeppermintOS-amd64.iso]
disk image  [peppermint.qcow2]
MAC address [52:54:c2:a8:66:f8]
```

Boots from the `.iso` and also provides the `.qcow2` disk image or asks to create it first.

<br>

#### 4.
```
$ qvm test -cdrom siduction-22.1-Masters_of_War-xfce-amd64-202212291715.iso
Configuration file: /home/you/.config/qvm/test

VM name     [test]
MAC address [52:54:b8:d7:d7:ab]
```

Boots from the `.iso` file specified on command line and uses the existing `test` configuration file (no disk images configured inside).

<br>

#### 5.
```
$ qvm /tmp/test disk.qcow2
Configuration file: /tmp/test

VM name     [test]
MAC address [52:54:b8:d7:d7:ab]
```

Boots up the `.qcow2` disk image using configuration in custom path `/tmp/test`.
