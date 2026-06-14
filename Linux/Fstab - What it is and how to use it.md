**fstab** stands for file systems table. It's located in `/etc/fstab`. It contains the system's mount configuration that defines which filesystems get mounted where at boot time. 

```ad-bash
	/dev/pve/root / ext4 errors=remount-ro 0 1
	UUID=DB68-5BED /boot/efi vfat defaults 0 1
	/dev/pve/swap none swap sw 0 0
	proc /proc proc defaults 0 0
```

