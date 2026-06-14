## Package Management

```bash
# Update package index
sudo apt update

# Install package
sudo apt install <package> -y

# Install git
sudo apt update && sudo apt install git -y

# Remove package
sudo apt remove <package>

# Search for package
apt search <keyword>

# Show package info
apt show <package>
```

## File & Directory Operations

```bash
# Remove empty directory
rmdir <directory>

# Remove directory with contents
rm -r <directory>

# Remove directory (force, no prompts) - DANGEROUS
rm -rf <directory>

# Remove directory (interactive, safer)
rm -ri <directory>

# Change permissions (symbolic)
chmod u+rw <path>              # Add read+write for owner
chmod u=rw,g=r,o= <path>       # Set explicit permissions
chmod -R u+rw <path>           # Recursive

# Change permissions (octal)
chmod 640 <path>               # Owner: rw, Group: r, Others: none
chmod 755 <path>               # Owner: rwx, Group: rx, Others: rx
chmod 775 <path>               # Owner: rwx, Group: rwx, Others: rx

# Change ownership
sudo chown user:group <path>
sudo chown -R user:group <path>  # Recursive
```

## Remote File Transfer

```bash
# Copy file to remote server
scp /local/file user@host:/remote/path/

# Copy directory to remote server
scp -r /local/dir user@host:/remote/path/

# Sync files to remote server (better for large transfers)
rsync -avz --progress /local/path user@host:/remote/path/

# Rsync with sudo on remote (for restricted destinations)
rsync -avz --rsync-path="sudo rsync" /local/path user@host:/remote/path/

# Copy via temp location if permissions are an issue
scp file user@host:/tmp/ && ssh user@host 'sudo mv /tmp/file /final/destination/'
```

## Network Configuration

```bash
# Show network interfaces
ip addr show
ip a                           # Short form

# Add virtual IP (IP alias)
sudo ip addr add 192.168.1.100/24 dev eth0 label eth0:0

# Remove IP address
sudo ip addr del 192.168.1.100/24 dev eth0

# Show specific interface
ip addr show dev eth0

# Show routing table
ip route

# Show DNS configuration
resolvectl status
```

## Netplan (Ubuntu Network Config)

```bash
# Edit network config
sudo nano /etc/netplan/01-netcfg.yaml

# Validate configuration
sudo netplan --debug generate

# Apply network changes
sudo netplan apply

# Show current netplan config
cat /etc/netplan/*.yaml
```

## User & Group Management

```bash
# Add user to group
sudo usermod -aG <group> <user>

# Add current user to docker group
sudo usermod -aG docker $USER

# Refresh group membership (current shell only)
newgrp <group>

# Show current user's groups
groups

# Show specific user's groups
groups <username>
```

## Docker

```bash
# Install Docker
sudo apt update
sudo apt install docker.io -y

# Enable Docker at boot
sudo systemctl enable docker

# Start Docker service
sudo systemctl start docker

# Verify Docker installation
docker --version
docker run hello-world

# Docker Compose - start in background
docker compose up -d

# Docker Compose - view running containers
docker compose ps

# Docker Compose - view logs
docker compose logs -f
docker compose logs -f <service_name>

# Docker Compose - stop containers
docker compose stop

# Docker Compose - stop and remove
docker compose down

# Docker Compose - stop, remove, delete volumes
docker compose down -v 

# Docker Compose - restart services
docker compose restart 

# Check Docker Compose version
docker compose version
```

## System Control

```bash
# Reboot system
sudo reboot
sudo systemctl reboot

# Shutdown system
sudo shutdown now
sudo shutdown -h now           # -h for halt
sudo poweroff

# Scheduled shutdown (in 10 minutes)
sudo shutdown +10

# Scheduled shutdown with message
sudo shutdown +10 "System maintenance in 10 minutes"

# Shutdown at specific time (24-hour format)
sudo shutdown 23:30

# Cancel scheduled shutdown
sudo shutdown -c

# Halt system (stop but don't power off)
sudo halt

# Check system uptime
uptime
```

## Service Management (systemd)

```bash
# Start service
sudo systemctl start <service>

# Stop service
sudo systemctl stop <service>

# Restart service
sudo systemctl restart <service>

# Enable service at boot
sudo systemctl enable <service>

# Disable service at boot
sudo systemctl disable <service>

# Check service status
sudo systemctl status <service>
systemctl is-active <service>
systemctl is-enabled <service>

# View service logs
sudo journalctl -u <service>
sudo journalctl -u <service> -f  # Follow mode
```

## File Viewing & Editing

```bash
# View file contents
cat <file>

# View file with pagination
less <file>
more <file>

# View end of file
tail <file>
tail -f <file>                 # Follow mode (real-time)
tail -n 20 <file>              # Last 20 lines

# Edit file
nano <file>                    # Beginner-friendly
vi <file>                      # Vi/Vim editor
vim <file>                     # Vim editor

# Create/edit file in nano
sudo nano /path/to/file
```

## Process Management

```bash
# Show running processes
ps aux
ps aux | grep <process_name>

# Show process tree
pstree

# Show real-time process monitor
top
htop                           # Better interface (install: sudo apt install htop)

# Kill process by PID
kill <PID>
kill -9 <PID>                  # Force kill

# Kill process by name
pkill <process_name>
killall <process_name>

# Run command in background
<command> &

# Run command immune to hangups
nohup <command> &

# Bring background job to foreground
fg

# List background jobs
jobs

# Kill flatpak application
flatpak kill com.discordapp.Discord
```

## Disk & Filesystem

```bash
# Show disk usage
df -h                          # Human-readable

# Show directory size
du -sh <directory>

# Show directory contents with sizes
du -h <directory> | sort -h

# Show mounted filesystems
mount

# Check filesystem
sudo fsck /dev/sdX

# Show disk partition info
lsblk
sudo fdisk -l
```

## System Information

```bash
# Show OS information
cat /etc/os-release
lsb_release -a

# Show kernel version
uname -r
uname -a                       # All info

# Show CPU info
lscpu
cat /proc/cpuinfo

# Show memory info
free -h
cat /proc/meminfo

# Show hostname
hostname
hostnamectl

# Show current user
whoami

# Show logged-in users
who
w
```

## Search & Find

```bash
# Find files by name
find /path -name "filename"
find /path -iname "filename"   # Case-insensitive

# Find directories
find /path -type d -name "dirname"

# Find files modified in last 7 days
find /path -mtime -7

# Search file contents (grep)
grep "pattern" <file>
grep -r "pattern" /path        # Recursive
grep -i "pattern" <file>       # Case-insensitive
grep -v "pattern" <file>       # Invert match

# Search in command history
history | grep <keyword>
```

## Archives & Compression

```bash
# Create tar archive
tar -cvf archive.tar /path

# Create compressed tar archive (gzip)
tar -czvf archive.tar.gz /path

# Extract tar archive
tar -xvf archive.tar

# Extract compressed tar archive
tar -xzvf archive.tar.gz

# Create zip archive
zip -r archive.zip /path

# Extract zip archive
unzip archive.zip
```

## Common Flags Reference

- `-h` : Human-readable output
- `-r` or `-R` : Recursive operation
- `-v` : Verbose output (show details)
- `-f` : Force operation / specify file
- `-i` : Interactive mode (prompts)
- `-a` : Archive mode (for rsync/cp)
- `-z` : Compress during operation
- `-d` : Detach / daemon mode
- `-y` : Auto-yes to prompts

## Critical Safety Notes

⚠️ **DANGEROUS COMMANDS** - Triple-check before running:

- `rm -rf` : Deletes everything with no confirmation
- `sudo rm -rf /` : DO NOT RUN - Deletes entire system
- `chmod -R 777` : Makes everything world-writable (security risk)
- `dd` : Can wipe drives if you get the target wrong

## Pro Tips

```bash
# Chain commands (run second only if first succeeds)
command1 && command2

# Chain commands (run second regardless of first)
command1; command2

# Pipe output to another command
command1 | command2

# Redirect output to file (overwrite)
command > file.txt

# Redirect output to file (append)
command >> file.txt

# Redirect errors to file
command 2> errors.txt

# Redirect both output and errors
command &> output.txt

# Suppress all output
command > /dev/null 2>&1

# Edit last command in editor
fc

# Rerun last command
!!

# Rerun last command with sudo
sudo !!

# Use previous command's arguments
!$
```

## Quick Networking Checks

```bash
# Test connectivity
ping -c 4 8.8.8.8

# Test DNS resolution
nslookup google.com
dig google.com

# Show listening ports
sudo ss -tulpn
sudo netstat -tulpn

# Show active connections
ss -tan

# Trace route to host
traceroute google.com

# Show firewall rules (ufw)
sudo ufw status
sudo ufw status verbose

# Show firewall rules (iptables)
sudo iptables -L -n -v
```