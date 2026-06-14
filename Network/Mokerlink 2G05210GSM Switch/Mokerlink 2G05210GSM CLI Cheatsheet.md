## VERIFIED COMMANDS - Tested and Working

**Connection:** `telnet 172.16.0.117` (SSH exists but uses deprecated crypto, does not work on modern systems)  
**Default Credentials:** `admin` / `admin

**For how to enable Telnet see:** [[Enable Telnet on Mokerlink 2G05210GSM From Web UI]]

---

### **ALERT!** Command `ip dhcp` *crashes the managaement interface*

```ad-bug
title:`ip dhcp`*CRASH* **>>Critical Bug<<**

**Effect:** Crashes management interface (web + telnet)  
**Recovery:** Hard reboot (power cycle) - loses all unsaved config  
**Reason:** Firmware bug - DHCP server conflicts with management interface  
**Solution:** Use external DHCP server (router, Pi-hole, server)
```

---

## Mokerlink CLI Structure


## Basic Commands (VERIFIED)

### Help & Discovery

```bash
?                       # Show all available commands in current interface
show ?                  # Show all 'show' commands
<command> ?               # Show command syntax/options
```

### Mokerlink CLI Navigation

- `config` - Enter configuration mode
- `enable` 
	- `password` - Use clear text memory
	- `secret` - Use encrypted password
```bash
enable                  # User EXEC → Privileged EXEC
configure terminal      # Privileged EXEC → Global Config
exit                    # Go back one level
end                     # Global Config → Privileged EXEC
```

### Save Configuration (MOST IMPORTANT)

```bash
write memory            # ✓ WORKS - Save running to startup config
show startup-config     # Verify config was saved
```

**Note:** `copy running-config startup-config` syntax may not work - use `write memory`

## CLI Modes

```
User EXEC Mode          >        Basic commands, view-only
Privileged EXEC Mode    #        Full command access
Global Config Mode      (config)# System-wide configuration
```


---


### System Information

```bash
show version            # Firmware version, uptime
show running-config     # Current active configuration (RAM)
show startup-config     # Saved configuration (Flash)
show info               # Basic system information
show memory             # Memory statistics
show cpu                # CPU utilization
```



---

## Interface Management (VERIFIED WORKING)

### View Interfaces

```bash
show interfaces         # ✓ WORKS - All interface status
show interface status   # ✓ WORKS - Port status summary
show interfaces gigabitethernet 1/0/1  # Specific interface (syntax may vary)
```

### Configure Interface (Basic - Test Before Using)

```bash
configure terminal
interface gigabitethernet 1/0/1
 description "Uplink to Core Switch"
 speed auto             # auto/10/100/1000/2500
 duplex auto            # auto/half/full
 no shutdown            # Enable interface
 shutdown               # Disable interface
exit

# CRITICAL: Save after changes
end
write memory
```

**Note:** Interface configuration syntax may vary. Use `?` to discover exact syntax.

---

## VLAN Configuration (VERIFIED WORKING)

### View VLANs

```bash
show vlan              # ✓ WORKS - All VLANs and port assignments
show vlan id 10        # Specific VLAN (may work)
```

### Create/Manage VLANs

**RECOMMENDED: Use Web GUI for VLAN configuration, then save via CLI**

```bash
# Creating VLANs via CLI (syntax may vary)
configure terminal
vlan 10
 name Management
exit

# CRITICAL: Save after VLAN changes
write memory
```

### Port VLAN Assignment

**STRONGLY RECOMMENDED: Configure via Web GUI**

The CLI syntax for port/VLAN assignment varies and is poorly documented.  
Safer workflow:

1. Configure VLANs in Web GUI
2. Assign ports to VLANs in Web GUI
3. Save via CLI: `write memory`

---

## Management IP Configuration (VERIFIED WORKING)

### Change Management IP

```bash
configure terminal
ip address 172.16.0.117 255.255.255.0
ip default-gateway 172.16.0.1
exit
write memory
```

### View IP Configuration

```bash
show ip interface       # ✓ WORKS - Current IP settings
```

---

## Available Show Commands (FROM YOUR SWITCH)

```bash
show backup-config      # Backup configuration
show cable-diag         # Cable Diagnostics
show clock              # System time and date
show cpu                # CPU utilization
show debugging          # Debugging information
show eee                # EEE (Energy Efficient Ethernet) config
show errdisable         # Error Disable status
show fiber-transceiver  # Fiber port diagnostics
show flash              # Flash memory operations
show info               # Basic system information
show interfaces         # Interface status and configuration
show ip                 # IP information (has subcommands)
show lacp               # LACP configuration
show lag                # Link Aggregation Groups
show line               # Line configuration (console/vty)
show logging            # Log configuration and entries
show mac                # MAC address table
show management-vlan    # Management VLAN configuration
show memory             # Memory statistics
show mirror             # Port mirroring configuration
show poe                # Power Over Ethernet status
show privilege          # User privilege levels
show qos                # QoS configuration
show rate               # Rate limiting configuration
show running-config     # Current active configuration
show snmp               # SNMP information
show sntp               # SNTP (time sync) information
show spanning-tree      # STP information
show startup-config     # Saved configuration
show username           # Local user accounts
show users              # Currently logged in users
show version            # System hardware/software status
show vlan               # VLAN configuration
```

---

## IP Subcommands (VERIFIED AVAILABLE)

```bash
configure terminal
ip ?
  address          # IPv4 Address
  default-gateway  # Set default gateway IP address
  dhcp             # ⚠️ DANGEROUS - DO NOT USE
  http             # HTTP server configuration
  https            # HTTPS server configuration
  igmp             # IGMP Configuration
  ssh              # SSH configuration
  sub-address      # Sub IPv4 Address
  telnet           # Telnet daemon configuration
```

### Enable/Disable Management Services

```bash
configure terminal

# Enable/disable Telnet
ip telnet
# OR
no ip telnet

# Enable/disable HTTP
ip http
no ip http

# Enable/disable HTTPS
ip https
no ip https

# Enable/disable SSH (broken, but configurable)
ip ssh
no ip ssh

# ALWAYS SAVE
write memory
```

---

## ⛔ DHCP Configuration - DO NOT USE

### DHCP Server Feature is BROKEN

**DO NOT RUN:** `ip dhcp`

**What happens:**

1. Management interface crashes
2. Web GUI becomes unreachable
3. Telnet becomes unreachable
4. Switch requires hard reboot (power cycle)
5. All unsaved configuration is lost

**Why it's broken:** The `ip dhcp` command enables DHCP server, which conflicts with the management interface IP. This is a critical firmware bug.

**Alternative Solutions:**

- Use your router's DHCP server
- Use Pi-hole on Raspberry Pi
- Use pfSense/OPNsense firewall
- Use dedicated Linux server (dnsmasq, isc-dhcp-server)
- Use Windows Server DHCP role

**For reference only (DO NOT USE):**

```bash
# This is what SHOULD work but DOESN'T:
configure terminal
ip dhcp enable          # ⛔ CRASHES SWITCH
ip dhcp pool VLAN10     # ⛔ NOT AVAILABLE
```

**If you need DHCP relay** (forward DHCP requests to external server):

```bash
# This MAY work (untested) - use at your own risk
configure terminal
interface vlan 10
 ip helper-address 192.168.1.1  # External DHCP server
exit
write memory
```

---

## Link Aggregation (LACP) - USE WITH CAUTION

### View LAG Status

```bash
show lacp               # ✓ WORKS - LACP information
show lag                # ✓ WORKS - Link Aggregation Groups
```

### Create LAG (UNTESTED - Verify Before Using)

```bash
configure terminal
interface range gigabitethernet 1/0/1-2
 channel-group 1 mode active  # active=LACP, on=static
exit
write memory
```

**Recommendation:** Configure LAG via Web GUI, then verify with `show lag`

---

## Spanning Tree Protocol (STP)

### View STP Status

```bash
show spanning-tree      # ✓ WORKS - STP information
```

### Configure STP (UNTESTED)

```bash
configure terminal
spanning-tree mode rstp     # rapid-pvst/mstp/rstp
exit
write memory
```

**Recommendation:** Configure STP via Web GUI

---

## Quality of Service (QoS)

### View QoS Status

```bash
show qos                # ✓ WORKS - QoS configuration
```

### Configure QoS

**STRONGLY RECOMMENDED:** Use Web GUI for QoS configuration  
CLI syntax for QoS is undocumented and unreliable

---

## Port Mirroring (SPAN)

### View Mirror Configuration

```bash
show mirror             # ✓ WORKS - Mirror configuration
```

### Configure Port Mirror

**RECOMMENDED:** Use Web GUI  
CLI syntax varies and is poorly documented

---

## MAC Address Table (VERIFIED WORKING)

### View MAC Addresses

```bash
show mac                # ✓ WORKS - MAC address table
show mac ?              # See subcommands
```

**Note:** Subcommand syntax may differ from Cisco IOS

---

## Logging and Diagnostics (VERIFIED WORKING)

### View Logs

```bash
show logging            # ✓ WORKS - System logs
```

### Cable Diagnostics

```bash
show cable-diag         # ✓ WORKS - Cable diagnostics
```

### Fiber Transceiver Info

```bash
show fiber-transceiver  # ✓ WORKS - SFP/SFP+ module info
```

### PoE Status (if applicable)

```bash
show poe                # ✓ WORKS - Power over Ethernet status
```

---

## Management & Security

### View User Accounts

```bash
show username           # ✓ WORKS - Local user accounts
show users              # ✓ WORKS - Currently logged in users
show privilege          # ✓ WORKS - User privilege levels
```

### Change Password (UNTESTED)

```bash
configure terminal
username admin privilege 15 password NewPassword123
exit
write memory
```

**SAFER METHOD:** Change password via Web GUI, then save via CLI

### Management Service Control

```bash
configure terminal

# Enable/Disable Services
ip telnet               # Enable telnet
no ip telnet            # Disable telnet

ip http                 # Enable HTTP
no ip http              # Disable HTTP

ip https                # Enable HTTPS  
no ip https             # Disable HTTPS

ip ssh                  # Enable SSH (broken crypto)
no ip ssh               # Disable SSH

exit
write memory
```

### SSH Configuration (BROKEN - Don't Bother)

SSH on this switch uses deprecated `ssh-rsa` and `ssh-dss` algorithms.  
Modern SSH clients (OpenSSH 8.8+) reject these by default.

**If you must use SSH:**

```bash
ssh -o HostKeyAlgorithms=+ssh-rsa -o PubkeyAcceptedKeyTypes=+ssh-rsa admin@172.16.0.117
```

**Reality:** Just use Telnet. SSH's security advantage is nullified by ancient crypto.

### SNMP Configuration

```bash
show snmp               # ✓ WORKS - SNMP information
```

**RECOMMENDED:** Configure SNMP via Web GUI

---

## Troubleshooting (VERIFIED WORKING)

### Connectivity Tests

```bash
ping 192.168.1.1        # ✓ WORKS
# Note: ping options may be limited
```

### System Information

```bash
show version            # ✓ WORKS - Firmware, uptime, hardware
show cpu                # ✓ WORKS - CPU utilization
show memory             # ✓ WORKS - Memory statistics
show clock              # ✓ WORKS - System time
```

### Interface Diagnostics

```bash
show interfaces         # ✓ WORKS - All interface status
show cable-diag         # ✓ WORKS - Cable diagnostics
show fiber-transceiver  # ✓ WORKS - SFP/SFP+ module info
```

### Configuration Verification

```bash
show running-config     # ✓ WORKS - Current config (RAM)
show startup-config     # ✓ WORKS - Saved config (Flash)
show flash              # ✓ WORKS - Flash memory status
```

### Reboot Switch

```bash
reload                  # Reboot switch
# You'll be prompted to save config if unsaved
```

### Factory Reset

**Method 1: Hardware Reset**

```
1. While switch is powered on
2. Hold reset button for 5+ seconds
3. All LEDs will flash
4. Release button
5. Switch resets to 192.168.2.1
```

**Method 2: CLI Reset (UNTESTED)**

```bash
write erase             # Erase startup config
reload                  # Reboot to defaults
```

---

## Configuration Backup & Restore

### Backup Configuration

```bash
show startup-config
# Copy output to text file manually

# OR via Web GUI:
# System → Backup/Restore → Download Configuration
```

### Restore Configuration

**Method 1: Web GUI** (RECOMMENDED)

```
1. System → Backup/Restore
2. Upload saved config file
3. Apply configuration
4. Telnet in and run: write memory
```

**Method 2: Manual CLI Entry**

```bash
telnet 172.16.0.117
enable
configure terminal
# Paste configuration commands one by one
exit
write memory
```

---

## Common Tasks Quick Reference

|Task|Commands|Notes|
|---|---|---|
|**Check port status**|`show interfaces`|✓ Works|
|**View VLANs**|`show vlan`|✓ Works|
|**Save config**|`write memory`|✓ REQUIRED AFTER EVERY CHANGE|
|**View running config**|`show running-config`|✓ Works|
|**View saved config**|`show startup-config`|✓ Works|
|**Change management IP**|`ip address 172.16.0.117 /24`|✓ Works|
|**Set gateway**|`ip default-gateway 172.16.0.1`|✓ Works|
|**View system info**|`show version`|✓ Works|
|**View MAC table**|`show mac`|✓ Works|
|**View logs**|`show logging`|✓ Works|
|**Reboot switch**|`reload`|✓ Works|
|**Enable telnet**|`ip telnet`|✓ Works|
|**Configure VLANs**|Use Web GUI + CLI save|Recommended|
|**Configure DHCP**|⛔ DON'T - Use external server|Crashes switch|

---

## Critical Workflow Reminders

### Every Time You Configure Anything:

```
┌──────────────────────────────────────────────┐
│ MANDATORY CONFIGURATION WORKFLOW:            │
├──────────────────────────────────────────────┤
│ 1. Make changes (Web GUI or CLI)            │
│ 2. Telnet to switch                          │
│ 3. Run: enable                               │
│ 4. Run: write memory                         │
│ 5. Verify: show startup-config               │
│ 6. Test: Power cycle switch (first time)    │
└──────────────────────────────────────────────┘

⚠️  Web GUI "Save" button DOES NOT WORK
⚠️  Config is lost on reboot without 'write memory'
⚠️  Never run 'ip dhcp' - crashes management
```

---

## What Works vs. What Doesn't

### ✅ VERIFIED WORKING

- Basic show commands (version, interfaces, vlan, mac, etc.)
- Configuration save via `write memory`
- Management IP configuration
- Telnet enable/disable
- VLAN viewing
- Interface status monitoring
- System diagnostics (cpu, memory, logs)
- Cable diagnostics
- Fiber transceiver info
- Switch reboot
- Factory reset

### ⚠️ USE WEB GUI FOR THESE

- VLAN creation and port assignment
- Port mirroring (SPAN)
- QoS configuration
- Link aggregation (LACP)
- STP configuration
- Password changes
- Complex ACLs

### ⛔ BROKEN - DO NOT USE

- `ip dhcp` - Crashes management interface
- Web GUI "Save" button - Doesn't save to flash
- SSH - Uses deprecated crypto (ssh-rsa/ssh-dss)
- DHCP server - Broken firmware implementation

### ❓ UNTESTED / UNRELIABLE

- Advanced interface configuration
- Static routes
- ACL configuration via CLI
- SNMP configuration via CLI
- User management via CLI

**Best Practice:** When in doubt, configure via Web GUI and save via CLI `write memory`

---

## Troubleshooting Guide

### Problem: Config Lost After Reboot

**Cause:** Didn't run `write memory` via CLI  
**Solution:** Web GUI save doesn't work. Always use: `enable` → `write memory`

### Problem: Can't Access Switch After Config Change

**Cause:** Changed management IP without updating your computer's IP  
**Solution:** Set your computer to same subnet as new switch IP

### Problem: Switch Crashed / Unresponsive

**Cause:** Ran `ip dhcp` command  
**Solution:** Hard reboot (power cycle). Never use `ip dhcp` command.

### Problem: SSH Won't Connect

**Cause:** Switch uses deprecated ssh-rsa/ssh-dss algorithms  
**Solution:** Use telnet instead, or force old SSH: `ssh -o HostKeyAlgorithms=+ssh-rsa admin@IP`

### Problem: VLAN Configuration Not Working

**Cause:** CLI syntax is poorly documented  
**Solution:** Use Web GUI for VLAN config, then `write memory` via CLI

### Problem: Commands Not Found / Syntax Error

**Cause:** This switch uses custom CLI, not full Cisco IOS  
**Solution:** Use `?` to discover available commands. When stuck, use Web GUI.

---

## Learning Resources

### Official Documentation

- **Mokerlink Support:** http://www.mokerlink.com/support/
- **Download Page:** May have firmware updates or manuals

### Community Resources

- **ServeTheHome:** Search "Mokerlink 2G05210GSM" for detailed reviews
- **OpenWrt Forums:** https://forum.openwrt.org/t/hasivo-switches/151758
    - This switch is a rebranded Hasivo - same firmware bugs/quirks
- **Reddit:** r/homelab, r/networking - search for Mokerlink/Hasivo issues

### Similar Switches (Same Firmware)

- Hasivo S600W series
- Hasivo S600WP series
- Various Chinese rebrands on AliExpress

**They all share the same bugs:**

- Web GUI save doesn't work
- DHCP crashes management
- SSH uses ancient crypto

---

## Notes & Disclaimers

### About This Switch

- **Model:** Mokerlink 2G05210GSM
- **Ports:** 5x 2.5GbE + 2x 10G SFP+
- **Chipset:** Realtek-based
- **CLI Style:** Cisco IOS-like (but limited/modified)
- **Firmware:** Varies by batch - check `show version`

### About This Cheatsheet

- **Status:** Based on actual testing with this switch model
- **Verification:** Commands marked ✓ have been tested and work
- **Warnings:** Commands marked ⛔ are confirmed broken
- **Cautions:** Unmarked commands may work but are untested

### General Advice

- **Save early, save often:** Run `write memory` after every change
- **Test reboots:** After major config changes, test with power cycle
- **Use Web GUI for complex tasks:** CLI documentation is poor/nonexistent
- **Backup configs regularly:** Download backup via Web GUI
- **Keep expectations realistic:** This is an $80 switch, not enterprise gear
- **When in doubt, ask:** Use `?` liberally to discover syntax

### Success Criteria for Budget Switches

✅ Good for: VLANs, port isolation, basic trunking, homelab learning  
✅ Acceptable for: Small office, home network segmentation  
❌ Not suitable for: Production environments, complex routing, critical infrastructure  
❌ Don't expect: Enterprise features, reliable documentation, vendor support

---

**Switch Model:** Mokerlink 2G05210GSM (Hasivo rebrand)  
**CLI Type:** Modified Cisco IOS-style  
**Management IP:** 172.16.0.117 (your configuration)  
**Last Updated:** February 2025  
**Cheatsheet Version:** 2.0 - Verified Commands Only