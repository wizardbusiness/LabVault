#### How to add, remove and manage network shares in Proxmox
### Adding a Share to the Proxmox Host With `pvesm`

##### **When to do this:** You want proxmox to manage your share 
##### A) NFS Share

```ad-bash
	pvesm add nfs pbs-nfs \
	  --server 192.168.1.100 \
	  --export /mnt/pbs-datastore \
	  --content snippets

**Decomposition**  
- `pvesm` - Proxmox VE Storage Manager binary (located at `/usr/sbin/pvesm`)
- `add` - Subcommand to create a new storage entry
- `nfs` - Storage plugin type (others: `dir`, `lvm`, `cifs`, `zfspool`, etc.)
- `pbs-nfs` - Storage ID, your arbitrary name for this storage in Proxmox's config
- `\` - Line continuation character (purely cosmetic, lets you split command across lines)
- `--server` - Parameter flag specifying NFS server location
- `192.168.1.100` - IP address of NFS server (could also be hostname/FQDN)
- `--export` - Parameter flag for the NFS export path
- `/mnt/pbs-datastore` - The full path that's exported on the NFS server (must match server's `/etc/exports` entry)
- `--content snippets` - `--content` flag tells Proxmox what types of data are allowed to be stored on this storage backend. It's a whitelist. `snippets` only allows scripts, configs, and user data to be saved to the share, no vm images, templates or backups created with proxmox's inbuilt backup system (not the same as proxmox backup server)
```

##### B) SMB Share

```ad-bash
	pvesm add cifs truenas-smb --server 192.168.1.100 --share myshare --username yourusername --password 'YourPassword'
	
**Decomposition**
- `pvesm` - Proxmox VE Storage Manager CLI tool
- `add` - Subcommand to add a new storage backend
- `cifs` - Storage type (Common Internet File System, aka SMB/Samba)
- `truenas-smb` - Your chosen storage ID (arbitrary name you'll reference later)
- `--server 192.168.1.100` - IP address of the CIFS/SMB server
- `--share myshare` - Name of the SMB share on that server
- `--username` - Username for accessing the SMB share on that server
- `--password` - Password string for accessing the SMB Share on that server
```

### Adding a Share to Proxmox Host via Direct Mount 

##### B) SMB Share

```ad-bash
	# open /etc/fstab
	nano /etc/fstab
```


Add the following to fstab file
```ad-bash
	//truenas-ip/share /mnt/shares/truenas-proxmox-smb cifs credentials=/root/.smbcredentials,uid=1000,gid=1000,file_mode=0775,dir_mode=0775,_netdev 0 0

**Column 1: `//truenas-ip/share`**

- The source/device to mount
- `//` - SMB/CIFS UNC path prefix (Windows-style)
- `truenas-ip` - Hostname or IP of the SMB server
- `/share` - Share name on the server (what you'd see in `\\server\share` on Windows)

**Column 2: `/mnt/shares/truenas-proxmox-smb`**

- Mount point - where this filesystem appears in the local directory tree
- Must exist before mounting (create with `mkdir -p`)

**Column 3: `cifs`**

- Filesystem type
- `cifs` - Common Internet File System (SMB protocol implementation in Linux)
- Tells mount which driver to use

**Column 4: Mount options (comma-separated, no spaces)**

`credentials=/root/.smbcredentials`

- Path to file containing SMB username/password
- Keeps secrets out of fstab (which is world-readable)
- File format: `username=foo` and `password=bar` on separate lines	
  
`uid=1000`
- Force all files to appear owned by UID 1000 on the host
- Without this, files might show as UID 0 or some arbitrary Windows SID mapping
- Critical for container bind mounts - this 1000 maps 1:1 into your container
  
`gid=1000`
  
- Force all files to appear owned by GID 1000 on the host
- Same reasoning as uid
  
`file_mode=0775`
- Force permission bits for files
- `0775` = `rwxrwxr-x` = owner/group full access, others read+execute
- Overrides whatever permissions the SMB server reports
- `0` prefix means octal notation
  
`dir_mode=0775`
- Force permission bits for directories
- Same as file_mode but for directories
- Directories need execute bit to be traversable
  
`_netdev`
- Mark as network device
- Tells systemd/init to wait for network before mounting
- Prevents boot failures if network isn't up yet
- Critical for network filesystems
  
**Column 5: `0`**

- Dump frequency (for the `dump` backup utility)
- `0` = don't dump/backup this filesystem
- Mostly obsolete, but still required in fstab syntax

**Column 6: `0`**

- Fsck pass number (filesystem check order at boot)
- `0` = don't run fsck on this filesystem
- Only meaningful for local filesystems
- Network filesystems always use `0` 

```


### Mounting a Share to a Container

```ad-bash
	pct set <VMID> -mp0 /mnt/pve/<storage-name>,mp=/mnt/share 

**Decomposition**	
- `pct` - Proxmox Container Toolkit CLI 
- `set <VMID>` - Modify container with ID <VMID> 
- `-mp0` - Mount point slot 0 (can use mp0-mp255) 
- `/mnt/pve/<storage-name>` - Source path on Proxmox host where SMB is mounted 
- `mp=/mnt/share` - Destination mount point inside the container 

```


```ad-bash
	df -h | grep /mnt/pve

**Decomposition** 
- `df` - Disk free space (bytes default)
- `-h` - Flag to change value from bytes to human-readable format 
- `|` - Pipe operator - takes standard output (stdout) from the left command and passes it as standard input (stdin) to the right command 
- `grep` = "global regular expression print" - searches text line-by-line
- `/mnt/pve` = the search pattern (literal string match in this case)
```

### Unmounting a Share from the Proxmox Host

##### SMB & NFS Shares

```ad-bash
	pvesm remove <storage-id>

**Decomposition**	
- `pvesm` - Proxmox VE Storage Manager
- `remove` - Subcommand to delete a storage entry
- `<storage-id>` - The name given to the share during `add`. *Example:* `myshare-nfs`
```

### Unmounting a Share from a Proxmox LXC Container

```ad-bash
	pct set <CTID> --delete mp0
	
**Decomposition**
- `pct` - Proxmox Container Toolkit CLI
- `--delete mp0` - Remove mount point 0 (use mp1, mp2, etc. for other mount points)
```


