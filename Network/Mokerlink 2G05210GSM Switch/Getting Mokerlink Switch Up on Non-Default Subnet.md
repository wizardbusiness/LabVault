## Setting Up Mokerlink On My Home Network
---
## Prerequisites

- **Switch:** Mokerlink 2G05210GSM (5x 2.5GbE + 2x 10G SFP+)
- **Network:** 172.16.0.0/24 subnet
- **Target IP:** 172.16.0.117
- **Tools needed:**
    - Computer with Ethernet port
    - Ethernet cable
    - Telnet client (built into Windows/Mac/Linux)

---

## Step 1: Initial Connection

### 1.1 Physical Setup

```
1. Power on the switch
2. Connect your computer to ANY port on the switch (ports 1-5)
3. Wait 60 seconds for switch to boot
```

### 1.2 Configure Your Computer's IP

**Your computer needs to be on the default switch subnet (192.168.2.x) temporarily.**

**Windows:**

```
1. Control Panel → Network and Sharing Center → Change adapter settings
2. Right-click Ethernet adapter → Properties
3. Select "Internet Protocol Version 4 (TCP/IPv4)" → Properties
4. Use the following IP address:
   IP address:      192.168.2.100
   Subnet mask:     255.255.255.0
   Default gateway: (leave blank)
5. Click OK
```

**macOS:**

```
1. System Preferences → Network
2. Select Ethernet → Configure IPv4: Manually
   IP Address:      192.168.2.100
   Subnet Mask:     255.255.255.0
   Router:          (leave blank)
3. Click Apply
```

**Linux:**

```bash
sudo ip addr add 192.168.2.100/24 dev eth0
# (Replace eth0 with your actual interface name)
```

---


## Step 3: Enable Telnet and Save via CLI

### 3.1 Connect via Telnet


### 3.2 Login and Save Config

```
Username: admin
Password: admin

Smart WEB Managed> enable
Smart WEB Managed# write memory

Building configuration...
[OK]
```


---

## Step 4: Change Management IP to 172.16.0.117

### 4.1 Via Web Interface

```
1. Navigate to: Network → IP Configuration (or System → Management IP)
2. Set:
   IP Address:     172.16.0.117
   Subnet Mask:    255.255.255.0
   Default Gateway: 172.16.0.1 (your router/gateway IP)
3. Click "Apply"
```
![[Network/Mokerlink 2G05210GSM Switch/static/image.png]]

---

## Step 5: Reconfigure Your Computer

### 5.1 Change Computer Back to Your Network

**Put your computer back on the 172.16.0.x subnet.**

Important: If you connected the modem to your computer via ethernet port, but your computer is connected to your domain via wlan, you must set *that same port's* device interface to the 172.16.0.x subnet. Otherwise because that port is directly connected to the mokerlink switch, *not* your local network you will not be able to access the switch's admin interface until you plug it into an upstream device like your router.  

**Windows/macOS/Linux:**

- Either set static IP: 172.16.0.100/24
- Or enable DHCP to get IP from your router

---

## Step 6: Verify New IP Works

### 6.1 Test Web Access

```
URL: http://172.16.0.117
Username: admin
Password: admin
```

### 6.2 Test Telnet Access

```bash
telnet 172.16.0.117
```

### 6.3 Test Ping

```bash
ping 172.16.0.117
```

**All three should work. If not, see Troubleshooting below.**

---

## Step 7: Power Cycle Test (IMPORTANT)

### 7.1 Reboot Switch

```
Option 1: Unplug power, wait 10 seconds, plug back in
Option 2: Via telnet: reload
```

### 7.2 Verify Config Survived

```
1. Wait 60 seconds for boot
2. Access http://172.16.0.117
3. Check IP settings still show 172.16.0.117
```

**If it reset to 192.168.2.1:** You forgot to `write memory` via CLI.

---

## Step 8: Configure VLANs (Optional)

### 8.1 Create VLANs via Web GUI

```
1. Navigate to: VLAN → 802.1Q VLAN
2. Click "Add VLAN"
3. VLAN ID: 10
   VLAN Name: Management
4. Click "Apply"

Repeat for additional VLANs (e.g., VLAN 20 = Guest, VLAN 30 = IoT)
```

### 8.2 Assign Ports to VLANs

```
1. Navigate to: VLAN → Port Configuration
2. Select Port 3
3. Set:
   VLAN Mode: Access
   PVID: 10
4. Click "Apply"
```

### 8.3 Configure Trunk Port (Uplink)

```
1. Select Port 1 (uplink to router/firewall)
2. Set:
   VLAN Mode: Trunk
   Tagged VLANs: 1,10,20,30
   Untagged VLAN: 1
3. Click "Apply"
```

### 8.4 SAVE VIA CLI (CRITICAL)

```bash
telnet 172.16.0.117
# Login
enable
write memory
exit
```

**Repeat this after EVERY configuration change.**

---

## Step 9: Basic Security (Recommended)

### 9.1 Change Default Password

```
Via Web GUI:
1. System → User Management
2. Select 'admin' user
3. Change password
4. Click "Apply"

Via Telnet - SAVE THE CONFIG:
telnet 172.16.0.117
enable
write memory
```

### 9.2 Disable Unused Services

```
Via Web GUI:
1. System → Management → Services
2. Disable: HTTP (keep HTTPS only)
3. Keep Telnet enabled (SSH is broken on this switch)
4. Click "Apply"

Via Telnet - SAVE:
enable
write memory
```

---

## Configuration Workflow (MEMORIZE THIS)

```
┌──────────────────────────────────────────────┐
│ EVERY TIME YOU CHANGE SETTINGS:              │
├──────────────────────────────────────────────┤
│ 1. Make changes in Web GUI                   │
│ 2. Do NOT click "Save" in Web GUI (broken)   │
│ 3. Telnet to switch: telnet 172.16.0.117    │
│ 4. Login and run:                            │
│    enable                                    │
│    write memory                              │
│ 5. Verify: show startup-config               │
│ 6. Done - config will survive reboot         │
└──────────────────────────────────────────────┘
```

---

## Troubleshooting

### Problem: Can't Access 192.168.2.1

**Solution:**

```
1. Verify your computer IP is 192.168.2.100/24
2. Check Ethernet cable is connected
3. Try different port on switch (1-5)
4. Factory reset: Hold reset button 5+ seconds
```

### Problem: Can't Access 172.16.0.117 After IP Change

**Solution:**

```
1. Check your computer is on 172.16.0.x subnet
2. Verify gateway can route to 172.16.0.117
3. Try ping 172.16.0.117 first
4. If no ping, connect directly to switch and check IP via 192.168.2.1
```

### Problem: Config Lost After Reboot

**Solution:**

```
You forgot to run 'write memory' via CLI.
Web GUI "Save" button DOES NOT WORK.
Always save via telnet:
  enable
  write memory
```

### Problem: Telnet Won't Connect

**Solution:**

```
1. Enable Telnet via Web GUI first
2. Run 'write memory' via CLI to save the setting
3. Check port 23 is open: nmap -p 23 172.16.0.117
4. Verify no firewall blocking telnet
```

### Problem: Switch Reset to 192.168.2.1

**Solution:**

```
Config wasn't saved. Start over from Step 4 but remember:
ALWAYS RUN 'write memory' via CLI after changes.
```

---

## Quick Reference Commands

### Save Configuration (MOST IMPORTANT)

```bash
telnet 172.16.0.117
# Login
enable
write memory
```

### View Current Config

```bash
show running-config     # What's currently active
show startup-config     # What's saved to flash
show ip interface       # Current IP settings
```

### View VLANs

```bash
show vlan               # All VLANs
show vlan id 10         # Specific VLAN
```

### View Port Status

```bash
show interface status
show interface gigabitethernet 1/0/1
```

### Reboot Switch

```bash
reload
```

### Factory Reset (Last Resort)

```bash
# Via CLI:
write erase
reload

# Via Hardware:
Hold reset button 5+ seconds while powered on
```

---

## Known Issues & Warnings

### ⚠️ DO NOT USE: ip dhcp

**Symptom:** Running `ip dhcp` command crashes management interface  
**Effect:** Web GUI and Telnet become unreachable  
**Recovery:** Hard reboot (power cycle)  
**Solution:** Use external DHCP server (router, Pi-hole, etc.)

### ⚠️ Web GUI Save Button Does NOT Work

**Symptom:** Configuration lost after reboot despite clicking "Save"  
**Effect:** All settings revert to defaults on power cycle  
**Solution:** ALWAYS use CLI `write memory` after any change

### ⚠️ SSH Uses Deprecated Crypto

**Symptom:** SSH connection fails with "no matching host key type"  
**Effect:** Can't use SSH with modern OpenSSH clients  
**Solution:** Use Telnet instead (or force ssh-rsa: `ssh -o HostKeyAlgorithms=+ssh-rsa admin@172.16.0.117`)

---

## Post-Setup Checklist

- [ ] Switch accessible at 172.16.0.117
- [ ] Web GUI loads successfully
- [ ] Telnet works to 172.16.0.117
- [ ] Default password changed
- [ ] Configuration saved via `write memory`
- [ ] Power cycle tested - config persisted
- [ ] VLANs configured (if needed)
- [ ] Trunk port configured (if needed)
- [ ] All config changes saved via CLI

---

## Daily Operations

### Making Configuration Changes

```
1. Open http://172.16.0.117 in browser
2. Navigate to feature (VLAN, ports, etc.)
3. Make changes, click "Apply"
4. Open terminal: telnet 172.16.0.117
5. Run: enable → write memory
6. Verify: show startup-config
```

### Checking Switch Status

```bash
# Quick health check
telnet 172.16.0.117
enable
show version                    # Uptime, firmware
show interface status           # Port status
show vlan                       # VLAN config
show mac address-table          # Connected devices
```

### Backing Up Configuration

```bash
# Via web GUI:
System → Backup/Restore → Download Config

# Save file as: mokerlink-backup-YYYYMMDD.cfg
# Store safely - you'll need it if switch factory resets
```

---

## Support Resources

- **Official Support:** http://www.mokerlink.com/support/
- **ServeTheHome Review:** Search "Mokerlink 2G05210GSM STH"
- **Similar Hasivo Models:** This switch is rebranded Hasivo S600W series
- **OpenWrt Forums:** https://forum.openwrt.org/t/hasivo-switches/151758

---

**Switch Model:** Mokerlink 2G05210GSM  
**Ports:** 5x 2.5GbE + 2x 10G SFP+  
**Chipset:** Realtek-based  
**CLI:** Cisco IOS-style (with quirks)  
**Firmware:** Check `show version` for your specific version

**Last Updated:** February 2025