*How to create BOOTABLE MEDIA from an ISO image?*

*Question: WHy can't you just copy a .ISO image to a usb drive and boot from it?*
*Answer: Copying the .iso to the usb drive is a **file-system level operation**. Creating bootable media is a **block level operation***

*Question: What is the difference between a file-system level operation and a block level operation?*
*Answer: file system level ops leave the structure of the drive and its partitions intact and leave the iso packaged up as a file within that structure, while a block level operation removes the partition and file system structure of the drive and writes brand new ones from the unpacked content of the iso*

**In this scenario the iso is a blueprint, which is used to create the new structure of the usb drive and turn it into bootable media**

