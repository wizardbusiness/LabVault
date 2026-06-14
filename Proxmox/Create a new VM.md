#### *Shows: Creating a new VM in proxmox from the command line*

---
### 1. Create the VM with basic config
`qm create <id> --name <somename> --memory 8192 --cores 4 --net0 virtio,bridge=vmbr0`

### 2. Add boot disk (adjust size for your needs)
`qm set <same id> --scsi0 local-lvm:32`

### 3. Attach ISO
`qm set <same id> --ide2 /var/lib/vz/template/iso/<your-iso-here>.iso,media=cdrom`

### 4. Set boot order to try CD first
`qm set <same id> --boot "order=ide2;scsi0"`

### 5. Start the VM
`qm start <same id>

