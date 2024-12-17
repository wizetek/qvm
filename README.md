# qvm
**Quick/QEMU VM launcher**

Bash shell script to quickly create QEMU KVM host-CPU virtual machines and disk images with separate configuration for each.

<br>

## Installation

```
$ chmod +x qvm
```

<br>

## Usage

```
qvm <configfile> [-qemuopt1 -qemuopt2 -qemuopt3 ...]
```

Requires a configuration file as an argument and can take multiple [options](https://man.archlinux.org/man/qemu.1) to pass directly to `qemu`.

<br>

## Examples

#### 1.
```
$ qvm foo
/home/you/.config/qvm/foo: Configuration file not found
(C)reate config or (q)uit?
#######################################################
## qvm configuration
##
## NET= <boolean> | bridge0 | br0 | virbr0
##   yes: enable
##   no: no networking
## MAC= <empty> | random | 52:54:01:23:45:67
##   blank/unset: QEMU default
##   random: generated on startup
## ICH9= <boolean>
##   yes: Q35/ICH9 host chipset
##   no: i440FX/PIIX3
## OPT=
##   -hdb /path/to/seconddisk.img
##   -hdd fourthdisk.img
##   (-hdc conflicts with -cdrom)
##   -drive format=raw,media=cdrom,readonly,file=cd.iso
##   -nic model=virtio-net-pci
##   -device VGA,edid=on,xres=1366,yres=768
##   -device qxl-vga,ram_size_mb=256,vram_size_mb=256
##   ...and other qemu command options
##
#######################################################
## One variable per line and no spaces in front
## Values must be in (begin with) double quotes
## First conflicting instance wins, except
## OPT can be set multiple times
#######################################################
#
# Internal:
#
# Zero delay also disables startup prompt to edit config
#DELAY="1"
#NAME="Linux distro"
#ISO="linux.iso"
#IMG="disk.qcow2"
#RAW="yes"
CPU="2"
MEM="4G"
#ICH9="yes"
#AUDIO="yes"
# No network disables setting MAC
#NET="no"
#MAC="52:54:de:ad:be:ef"
#MAC="random"
#VNC=":2"
#TELNET="2302"
#SSH="2202"
#
# Passed to qemu:
#
# Fixes Windows guest mouse pointer issues
#OPT="-usbdevice tablet"
#
# Recommended for Linux guests instead of std VGA
#OPT="-vga qxl"
#
# For OpenGL, Wayland, etc.
#OPT="-device virtio-vga-gl -display gtk,gl=on"
#
# Misc (last one wins conflict)
#OPT="-display gtk,zoom-to-fit=on,window-close=off"
#OPT="-display sdl"
#OPT="-daemonize"

Saved in: /home/you/.config/qvm/foo
Edit this file and set IMG= and/or ISO=
```

Attempts to launch the virtual machine using the configuration file `foo` if available, otherwise only creates it.

<br>

#### 2.
`/home/you/.config/qvm/foo`
```
...
...
ISO="PeppermintOS-amd64.iso"
...
...
```

```
$ qvm foo
Configuration file: /home/you/.config/qvm/foo
Launching in 5... or (e)dit config?

VM name     [foo]
boot from   [PeppermintOS-amd64.iso]
MAC address [52:54:50:0e:39:ce]
```

Boots from the `.iso` image set in the configuration file. (Good for testing but not suitable for installation without a disk image attached – see below.)

<br>

#### 3.
`/home/you/.config/qvm/foo`
```
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
Launching in 5... or (e)dit config?
IMG=peppermint.qcow2: Inaccessible or not found
(P)repare disk image, (c)ontinue, (q)uit?
Disk image size? (50G) => 
File format? (qcow2) raw vdi vhdx vmdk => 
Name? (peppermint.qcow2) => 
(C)reate disk image, (r)edo, (q)uit?
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
Launching...

VM name     [test]
MAC address [52:54:b8:d7:d7:ab]
```

Boots from the `.iso` file specified on command line and uses the existing `test` configuration file (no disk images configured inside).

<br>

#### 5.
```
$ qvm /tmp/test disk.qcow2
Configuration file: /tmp/test
Launching...

VM name     [test]
MAC address [52:54:b8:d7:d7:ab]
```

Boots up the `.qcow2` disk image using configuration in custom path `/tmp/test`.

<br>

## Notes
* When a (hard) disk image is attached, booting from `.iso` happens only once on initial VM startup so that after the VM is reset it can boot from disk instead of going in a loop.
* Spaces in file names are supported for `ISO=` and `IMG=` defined in configuration files but not when specified on command line.
* UEFI boot `OPT="-bios /usr/share/ovmf/x64/OVMF.fd"`
