# swapfile and zswap

Customize swappiness:

swappiness is delay of return of "old" pages from swap.

ratio controls decision "swap" or "drop"(if 0 then drop, if 100 then swap?)?
```
$ cat /etc/sysctl.conf

vm.swappiness = 75
#vm.
vm.dirty_writeback_centisecs=1500
vm.dirty_expire_centisecs=4000
vm.dirty_background_ratio=25
vm.dirty_ratio=25
```
Add and enable compressor:
```
$ cat /etc/default/grub 

GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="rhgb quiet zswap.enabled=1 zswap.compressor=lzo"
GRUB_DISABLE_RECOVERY="true"
```
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

Use swapfile:
```
swapon --show
fallocate -l 2048M /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```
```
$ cat /etc/fstab
...
/swapfile none swap defaults,noatime 0 0
```

https://wiki.archlinux.org/index.php/Zswap

swapfile
https://wiki.archlinux.org/index.php/swap

goo.gl/OWiIBs

I don't recommend to use lz4.
"better is the enemy of good"

If HDD supports NCQ then set elevator=deadline as kernel option.
