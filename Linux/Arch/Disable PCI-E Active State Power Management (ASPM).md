**Description: ASPM is a PCIe power-saving feature that puts cards into low-power states when allowed by current system utilization**

**When might this be useful?**
*Troubleshooting connectivity or stability issues for PCI devices*

**When I've used this**
 - *Troubleshooting wifi cutting out intermittently with Mediatek Wifi m.2 PCIE Wifi Card*
	- ***Did it fix the issue?** No, i ended up getting a new wifi card*


```ad-warning
**This will disable power saving on ALL PCIE devices and increase power consumption**
```

1. Edit the grub config file with sudo privileges 
```ad-bash
	sudo nano /etc/default/grub
```

2. Find the line with `GRUB_CMDLINE_LINUX_DEFAULT` and add:
```ad-bash
	pcie_aspm=off
```

Edited line example:

```ad-bash
title: bash `snippet`
	GRUB_CMDLINE_LINUX_DEFAULT="quiet splash pcie_aspm=off"
```


3. Save the configuration - `Ctrl + O` and exit `Ctrl + E`
4. Reboot the system 
```ad-bash
title: bash `snippet`
	sudo reboot
```

**Additional Info**

As of 12/17/25 Mediatek wifi cards have notorious connection stability issues in linux and this is one of the suggested fixes. 