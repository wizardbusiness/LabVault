
**Bind Mounts:** A bind mount makes a directory from your Proxmox host directly accessible inside a container. VM's do not use bind mounts, only containers do. 

##### *When to use:* You need a container to have access to the contents of a folder/ share which is mounted to the proxmox host.  

```ad-bash

	pct set <CTID> -mp0 /mnt/pve/<storage-name>,mp=/mnt/nfs

	
**Decomposition**
- `pct` - base-command for Proxmox Container Toolkit (LXC Container Manager) 
- `pct set` = Modify container configuration
- `<CTID>` = Container ID (*e.g., 100, 101*)
- `-mp0` = Mount point slot (can use mp0 through mp255)
- `/mnt/pve/<storage-name>` = Host-side path where NFS is mounted (pvesm mounts to `/mnt/pve/` by default)
- `,mp=` = Separator for container-side mount point
- `/mnt/nfs` = Where it appears inside the container
  
```

