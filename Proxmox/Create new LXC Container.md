#### Before you start: Make sure you actually want to use a container and not a VM (Expand) 
##### Proxmox supports both VM's and LXC Containers. Both have advantages in different usecases

![[When to use a VM vs a Container#Use a Container when]]

![[When to use a VM vs a Container#Use a VM when]]

https://claude.ai/share/cc10ebee-039b-4643-a0d0-970cd30de21a




### 1. Download a container template 

```ad-bash
	pveam update
**Decomposition**
- `pveam` - Proxmox VE Appliance Manager (template manager)
- `update` - refresh the list of available templates
```


```ad-bash
	pveam available
**Decomposition**
- `pveam`- Proxmox VE Appliance Manager (template manager)
- `available` - show all templates you can download

```


```ad-bash
	pveam download local ubuntu-22.04-standard_22.04-1_amd64.tar.zst

**Decomposition**

- `pveam` = Proxmox VE Appliance Manager (template manager)
- `download` = fetch template
- `local` = storage location (usually `/var/lib/vz/template/cache`)
- `ubuntu-22.04-standard_22.04-1_amd64.tar.zst` = specific template name from the `available` list
```

### 2. Create the container

```ad-bash
	pct create 100 local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst \
	  --hostname my-container \
	  --memory 2048 \
	  --cores 2 \
	  --rootfs local-lvm:8 \
	  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
	  --password

Decomposed:

- `pct` = Proxmox Container Toolkit
- `create` = create new container
- `100` = CT ID (container identifier, like a VM ID)
- `local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst` = storage:path to template
- `--hostname my-container` = sets /etc/hostname inside container
- `--memory 2048` = RAM in MB
- `--cores 2` = CPU cores allocated
- `--rootfs local-lvm:8` = storage location : disk size in GB
- `--net0` = first network interface (net1, net2, etc. for additional)
    - `name=eth0` = interface name inside container
    - `bridge=vmbr0` = Proxmox bridge to connect to
    - `ip=dhcp` = get IP via DHCP (or use `ip=192.168.1.100/24,gw=192.168.1.1`)
- `--password` = prompts you to set root password

```

### 3. Start it

```bash
pct start 100
```

- `start` = boot the container
- `100` = your CT ID

### 4. Access it

```bash
pct enter 100
```

- `enter` = execute console login into container (like `docker exec -it`)

**Additional thoughts:**

