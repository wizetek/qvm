# qvm
Quick/QEMU VM launcher

#### Usage examples:

```
$ qvm pepper.qvm
```
Use settings in `pepper.qvm` to launch VM. Create a new config file on first use. 
<br><br>
```
$ vim pepper.qvm
ISO="PeppermintOS-amd64.iso"
IMG=pepper.qcow2

$ qvm pepper.qvm
```
Use disk image `pepper.qcow2` if available, otherwise prompt to create an empty disk. Boot from ISO image first.
