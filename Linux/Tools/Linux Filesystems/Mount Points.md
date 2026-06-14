What's a mount point in Linux/Unix, and how does it differ from a regular directory in the filesystem hierarchy?

**Answer:**

A mount point is a directory that serves as an attachment point for a filesystem. When you mount a filesystem, you're grafting an entire separate filesystem tree onto your existing directory structure at that specific location.

**What it IS:**

- A regular directory that acts as a connection point
- The entry point where a separate filesystem becomes accessible
- A logical "door" between your root filesystem and another storage device/partition/network share

**What it is NOT:**

- Not special in structure (it's just a normal directory before and after unmounting)
- Not the filesystem itself (it's just where the filesystem attaches)
- Not containing the actual data (the data lives on the mounted device/filesystem)

**Key difference from normal directories:**

bash

```bash
# Normal directory - part of the parent filesystem
/home/user/documents/  # Lives on whatever filesystem /home is on

# Mount point - connection to a DIFFERENT filesystem
/mnt/external_drive/   # Directory exists on root filesystem
                       # BUT when mounted, shows contents from /dev/sdb1
```

**Practical example breakdown:**

bash

```bash
# Create a directory (just a normal directory)
mkdir /mnt/backup

# Check what's using it
df -h /mnt/backup
# Output: Shows it's part of root filesystem (/)

# Mount a device to it
mount /dev/sdb1 /mnt/backup

# Now check again
df -h /mnt/backup
# Output: Shows /dev/sdb1 filesystem, different size/usage

# What happened:
# - /mnt/backup still exists as a directory
# - But now it's a "window" into /dev/sdb1's filesystem
# - Any files that WERE in /mnt/backup are hidden (shadowed)
# - You see /dev/sdb1's root directory contents instead
```

**The shadowing behavior:**

bash

```bash
# Before mount
echo "test" > /mnt/backup/file.txt
ls /mnt/backup/
# Shows: file.txt

# After mounting /dev/sdb1 to /mnt/backup
mount /dev/sdb1 /mnt/backup
ls /mnt/backup/
# Shows: whatever is on /dev/sdb1 (file.txt is hidden but not deleted)

# After unmount
umount /mnt/backup
ls /mnt/backup/
# Shows: file.txt again
```

**Additional thoughts:**

This concept is crucial for understanding container filesystems, LXC/Docker storage, and Proxmox's directory structure. In Proxmox specifically:

- `/var/lib/vz` is often a mount point for VM/CT storage
- Each LXC container sees mount points for bind mounts you configure
- Understanding mount points helps you grasp why running out of space on one partition doesn't affect others

The abstraction here is powerful: your entire filesystem appears as one tree, but it's actually composed of multiple filesystems stitched together at mount points. This is why you can have `/` on an SSD, `/home` on a large HDD, and `/mnt/nas` on a network share—all appearing as one unified directory structure.