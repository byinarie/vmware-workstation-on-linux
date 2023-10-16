# vmware-workstation-on-linux
common kernel / vmware-modconfig problems and potential fixes

# references for 16+
- https://communities.vmware.com/t5/VMware-Workstation-Pro/VM-Workstation-16-1-gt-16-2-1-on-Ubuntu-21-10-broke-everything/m-p/2884738#M173158
- https://communities.vmware.com/t5/VMware-Workstation-Pro/solved-vmware-modconfig-is-unable-to-install-modules-on-Linux/td-p/2913806
- https://communities.vmware.com/t5/VMware-Workstation-Pro/VMware-16-2-3-not-working-on-Ubuntu-22-04-LTS/m-p/2905535#M175399
- https://github.com/aircrack-ng/rtl8188eus/issues/263

# vmmon or vmnet compile error fix (16.x.x - 17.x.x)
Modify the below script for your EXACT version you're attempting to install, specifically line `git checkout workstation-16.2.3`
```
cd /usr/lib/vmware/modules/source
git clone https://github.com/mkubecek/vmware-host-modules
cd vmware-host-modules
git checkout workstation-16.2.3
make
tar -cf vmnet.tar vmnet-only
tar -cf vmmon.tar vmmon-only
mv vmnet.tar /usr/lib/vmware/modules/source/
mv vmmon.tar /usr/lib/vmware/modules/source/
vmware-modconfig --console --install-all
```

# /bin/sh: 1: ./tools/bpf/resolve_btfids/resolve_btfids: not found
1. Open your kernel headers Makefile.modfinal file for your kernel
2. uname -r     
`6.1.0-kali9-amd64`
3.  Example: /usr/src/linux-headers-6.1.0-kali9-amd64/scripts/Makefile.modfinal
4.  Locate and delete the following lines:
```
ifdef CONFIG_DEBUG_INFO_BTF_MODULES
	+$(if $(newer-prereqs),$(call cmd,btf_ko))
endif
```
4. run: `sudo vmware-modconfig --console --install-all`
5. look for errors and try running workstation again

# kernel headers missing or similar
`sudo apt install linux-headers-$(uname -r)`

# missing compilation programs
```sudo apt update; sudo apt install build-essential gcc make perl dkms dwarves```


# segfault or similar on workstation 15
likely not going to be a fix, see:
- https://communities.vmware.com/t5/VMware-Workstation-Pro/vmware-15-5-7-line-105-segmentation-fault-core-dumped/td-p/2828382
- https://communities.vmware.com/t5/VMware-Workstation-Pro/Linux-5-8-host-support-like-VirtualBox-this-year-or-next/m-p/2296583

its really not worth messing with, just use 16+

