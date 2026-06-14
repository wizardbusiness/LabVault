#### Use a Container when:
- **Microservices/web apps** - Nginx, databases, application servers, APIs
- **High density** - Running 20+ similar workloads on one host
- **Fast provisioning** - Need to spin up/down in seconds, not minutes
- **Development environments** - Disposable testing environments for userspace software
- **Stateless services** - Don't need hardware passthrough or kernel independence
- **Resource efficiency** - Limited RAM/CPU, every MB counts

#### Use a VM when:
- **Non-Linux OS** - Windows, BSD, anything not Linux
- **Kernel-level requirements** - Need custom kernel, kernel modules, eBPF programs, or specific kernel version
- **Maximum isolation** - Untrusted workloads, multi-tenant environments, security boundaries matter
- **Desktop environments** - Running GUI applications with hardware acceleration
- **Legacy applications** - Expects full init system behavior, systemd quirks, or specific kernel interfaces
- **Snapshots with kernel state** - Need to capture complete system state including kernel

#### Additional Thoughts
##### LXC vs VM decision pattern: Use LXC when you need multiple isolated Linux userlands sharing the same kernel (fast, lightweight, efficient). Use VMs when you need different kernels, non-Linux OS, or full hardware emulation.

##### The storage model matters: `local` is directory-based (good for templates), `local-lvm` is LVM-thin (good for container/VM disks with snapshots). Understanding Proxmox storage types (`dir`, `lvm`, `lvm-thin`, `zfs`, `ceph`) prevents you from fighting the system later when you want snapshots or live migration.
