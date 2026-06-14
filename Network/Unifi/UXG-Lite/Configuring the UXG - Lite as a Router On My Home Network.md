Resources
https://www.reddit.com/r/Ubiquiti/comments/1aq3ojs/confused_with_the_gateway_lite_uxglite/


### Task

##### ***Use To***  Text Here

#### Prerequisites
- [[<Internal Link Here>]]
#### Additional References
-  [[1] example.com reference alias][1]

[1]: www.example.com
#### See Also
- [[<Related Doc>]]

### *Info*
##### I'm adding a UXG-Lite to my home network which currently just has 2 deco tp-link consumer-grade aps/ router configured in a mesh. The goal is to put the uxg-lite in front of the isp modem in order to handle configuration and delegation. I have done some work with Unifi Products and am familiar with the web interface but have never set one up from scratch.

***

### Involved Components

- **2x Deco Tp-Link routers**
- **1x Modem**
- **1x UXG-Lite**
- **2x Homelab Servers**
	- **Proxmox Server**
	- **Truenas Scale Server**
- **1x Mokerlink Switch**
### Roadmap 

**Phase 1: Controller Setup**

- Spin up a `UniFi Network Controller` instance (you'll run this on one of your Proxmox VMs or as a container)
- The controller manages the UXG-Lite—without it, the gateway is a brick

**Phase 2: Physical Topology**

- Connect: ISP Modem (WAN) → UXG-Lite `WAN port` → UXG-Lite `LAN port` → Mokerlink switch
- Connect downstream devices to switch: 2x Deco units, 2x servers (Proxmox + TrueNAS)

**Phase 3: Gateway Adoption**

- Adopt the UXG-Lite into your controller
- Configure `WAN interface` (likely DHCP from ISP modem)
- Configure `LAN network` (your internal subnet, e.g., 192.168.1.0/24)

**Phase 4: Deco Reconfiguration**

- Disable `router mode` on both Decos—they must operate as `bridge mode APs`
- Configure identical `SSID/password` on both for seamless roaming
- Connect both via ethernet to switch (eliminates mesh wireless backhaul dependency)

**Phase 5: Homelab Access**

- Assign `static IPs` or `DHCP reservations` to your Proxmox and TrueNAS hosts
- Configure any necessary `firewall rules` in the UniFi controller to allow wireless clients to access homelab subnets
- Test wireless client connectivity to management interfaces (Proxmox web GUI, TrueNAS web GUI)

**Phase 6: Verification**

- Confirm internet access through UXG-Lite
- Confirm wireless clients can reach homelab services
- Confirm both Decos are providing coverage without routing conflicts

---

**Additional Thoughts:**

This setup separates your `control plane` (UniFi Controller managing the gateway) from your `data plane` (actual traffic routing). The UXG-Lite handles `NAT`, `DHCP`, `routing`, and `firewall functions` that your Deco was previously doing—this is called `single point of policy enforcement`.

Running the controller on Proxmox follows the `infrastructure as code` pattern—you can snapshot/backup your network config. Consider:

- Running the controller in a `privileged LXC container` or `Docker container` for easy maintenance
- Keeping the controller on a `management VLAN` (advanced—not required initially)
- The Decos in AP mode lose their mesh controller logic—they're now just `wireless bridges` to your wired network, which is actually more reliable

One architectural gotcha: if your controller VM goes down, the gateway keeps routing but you lose management capability. This is `control plane vs. data plane separation`—normal traffic continues, but you can't make config changes.

---

**Term Glossary:**

- `gateway deployment` - Installing a device that connects your LAN to the WAN and handles routing/NAT
- `downstream switching` - Switch placed on the LAN side of the gateway, not upstream
- `wireless backhaul access` - Using WiFi to reach wired infrastructure (your servers)
- `Layer 3 gateway` - Network device operating at the IP routing layer (OSI Layer 3)
- `UniFi Network Controller` - Ubiquiti's management software for UniFi devices (cloud key, self-hosted, or cloud-based)
- `AP mode` - Access Point mode; device only handles WiFi, not routing/DHCP
- `WAN port` - Wide Area Network port; connects to upstream internet source
- `LAN port` - Local Area Network port; connects to internal network
- `WAN interface` - Logical network interface facing the internet
- `LAN network` - Your internal subnet/IP address space
- `bridge mode APs` - Access points that pass traffic without routing (Layer 2 only)
- `SSID` - Service Set Identifier; your WiFi network name
- `static IPs` - Manually assigned IP addresses that don't change
- `DHCP reservations` - DHCP server always assigns same IP to specific MAC address
- `firewall rules` - ACLs (Access Control Lists) that permit/deny traffic flows
- `NAT` - Network Address Translation; how private IPs communicate with internet
- `DHCP` - Dynamic Host Configuration Protocol; automatically assigns IP addresses
- `control plane` - Management/configuration layer of network infrastructure
- `data plane` - Actual packet forwarding and traffic handling
- `single point of policy enforcement` - All security/routing rules configured in one place
- `privileged LXC container` - Linux container with elevated permissions in Proxmox
- `Docker container` - Lightweight application container using Docker runtime
- `management VLAN` - Separate virtual network for infrastructure management traffic
- `wireless bridges` - Devices extending wired network via WiFi without routing

---

### Proxmox LXC Container Setup 
**Step 1: Create the LXC Container**
In Proxmox web UI:

1. Click `Create CT` (top right)

2. Configure as follows:
    - **Hostname:** `unifi-controller` (or whatever makes sense to you)
    - **Password:** Set root password (you'll SSH in with this)
    - **Unprivileged container:** Uncheck. (we need privileged for network device access)
    - **Resource Pools:** Leave blank (default pool)
	- **SSH Public Key (Optional but Recommended)**
		- Load your SSH public key (the `.pub` file from your workstation)
			- `cat ~/.ssh/id_ed25519.pub`
		- Proxmox injects it into the container's `/root/.ssh/authorized_keys`
		- You SSH in without password: `ssh root@[CONTAINER-IP]`
		- Pros: More secure, convenient, industry standard
		- Cons: Requires you to have/generate SSH keys first

```ad-img
![[image.png|Create-LXC Container in Proxmox]]
```


- **Template:** Debian 12 (download from Templates if not already present)
	**Download Debian 12 Template** 
	
	In Proxmox web UI:
	
	1. Click on your **storage** in the left sidebar where templates are stored
	    - Usually named `local` (on the Proxmox node itself)
	    - Look for storage with "CT Templates" in the `Content` column
	
	2. Click on **CT Templates** (under the storage)
```ad-img
![[image-2.png]]
```
	
1. Click **Templates** button at the top
```ad-img
![[image-3.png]]
```

2. In the template list, find `debian-12-standard`
	- You can use the search box to filter: type "debian"
	- Latest Stable release as of 02/04/26

3. Click on `debian-12-standard` to select it

4. Click **Download** button at bottom
The `task viewer` will pop up showing download progress. Takes 30-60 seconds typically.
```ad-img
![[image-5.png]]
```
5. Once the task completes (Task OK) close the task viewer
```ad-img
![[image-6.png]]
```
6. You should see your downloaded template show up
```ad-img
![[image-7.png|962x282]]
```
	
- **Disk:** 16GB (plenty of headroom for logs/backups)
```ad-img
![[image-8.png]]
```
- **CPU:** 2 cores
```ad-img
![[image-9.png]]
```
- **Memory:** 2048 MB, Swap: 512 MB
```ad-img
![[image-10.png]]
```
- **Network:**
	- Bridge: `vmbr0` (your default bridge)
	- IPv4: Set static ip (or set DHCP now, and set static after adoption)
	- IPv6: `DHCP` or leave blank
	- Vlan Tag: Leave blank (not needed for flat network)
	- Mac Address: Auto (only need to set manually if you are cloning a container and need to match the original mac for DHCP reservation)
- **DNS** 
	- DNS Domain: use host settings
	- DNS Server use host settings
	
Uses same DNS configuration as the Proxmox host itself. Proxmox reads its own `/etc/resolv.conf` and injects those `nameserver` entries into the container's `/etc/resolv.conf` at creation.

```ad-img
![[image-12.png]]
```

- **Confirm:** Make sure that all the choices you entered are correct and
	- Check 'Start after created' to immediately start the container
	- Hit 'Finish'
```ad-img
![[image-13.png]]
```

- **Verify and close task view:** If task OK you can close the task view. Your container is now ready to use 
```ad-img
![[image-16.png]]
```
3. **Important:** After creating container - Under `Options` set:
    - `Start at boot`: `Yes` (so controller survives Proxmox reboots)
    - `Features`: Enable `keyctl` and `nesting` (required for some UniFi processes)
4. Start the container, then open console from Proxmox UI


**Step 2: Initial Container Setup**

Inside the container console:

```ad-bash
	apt update && apt upgrade -y
	
**Decomposition:**

- `apt update` - Refreshes the `package index` from Debian repositories
- `apt upgrade -y` - Installs all available package updates, `-y` flag auto-confirms
  
**Recomposition:** You always update before upgrade to ensure you're pulling the latest package versions from the repos.
```

Install Prerequisites

```ad-bash
	apt install curl wget gnupg2 software-properties-common ca-certificates apt-transport-https -y

**Decomposition**	
- `curl/wget` - Tools for downloading files from URLs
- `gnupg2` - Handles `GPG key verification` for repository signing
- `software-properties-common` - Provides `add-apt-repository` [[Command (Linux)#Definition|command]]
- `ca-certificates` - SSL certificate bundle for HTTPS connections
- `apt-transport-https` - Allows APT to fetch packages over HTTPS
  
**Recomposition:** Installs many packages simultaneously and auto accepts each install confirmation
```

**Step 3. Install MongoDB**

UniFi Controller requires MongoDB > 7.0.x (older versions won't work with current controller):

```ad-bash
	curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | gpg --dearmor -o /usr/share/keyrings/mongodb-server-8.0.gpg
	
**Decomposition:**

- `curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc` - Downloads the MongoDB `GPG signing key` (verifies package authenticity)
- `|` - Pipes output to next [[Command (Linux)#Definition|command]]
- `gpg --dearmor` - Converts the key to binary format APT can use
- `-o /usr/share/keyrings/mongodb-server-8.0.gpg` - Saves to the system's `keyring directory`
```

```ad-bash
	echo "deb [signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg] https://repo.mongodb.org/apt/debian bookworm/mongodb-org/8.0 main" | tee /etc/apt/sources.list.d/mongodb-org-8.0.list

**Decomposition:**
- `echo` - Prints the argument string to the terminal, and more importantly returns argument as stdout for piping 
- `deb [signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg]` - Creates the `APT source entry` for MongoDB repo
- `signed-by=` - Tells APT which GPG key to use for verification
- `bookworm` - Debian 12's codename
- `| tee /etc/apt/sources.list.d/` - Writes to the `sources.list` directory (where APT looks for repos)
	
**Recomposition:** Writes an APT source entry that tells APT to download MongoDB 8.0 packages from the official MongoDB repository, and to verify those packages are legitimately from MongoDB using the specified GPG key stored in the mongodb-server-8.0 gpg keyring.  
```

```ad-bash
	apt update
	apt install mongodb-org -y
	systemctl enable --now mongod
	
**Decomposition:**

- `apt update` - Refreshes package index (now includes MongoDB repo)
- `apt install mongodb-org -y` - Installs MongoDB 7.0
- `systemctl enable --now mongod` - Enables MongoDB to start at boot AND starts it immediately
	- **Note:** The `--now` flag combines `enable` (boot persistence) and `start` (immediate execution) into one [[Command (Linux)#Definition|command]].
	  
**Recomposition:** Update system packages, install monogdb, and enables and starts it immediately
```


**Step 4. Install Unifi Controller**

Reference: https://claude.ai/share/028a8880-ab45-4053-8a6c-96a202ab7dde

Get the ubiquiti gpg key from their offical repo and add to keychain 
```ad-bash
	curl -fsSL https://dl.ui.com/unifi/unifi-repo.gpg | gpg --dearmor -o /usr/share/keyrings/ubiquiti-archive-keyring.gpg

**Code decomposition:**

- `curl` - [[Command (Linux)#Definition|command]] to fetch data from a url 
- `curl -fsSL https://dl.ui.com/unifi/unifi-repo.gpg` - Downloads Ubiquiti's `GPG signing key` from their official server
  - `-fsSL`   
- `| gpg --dearmor` - Converts the ASCII-armored key to binary `GPG keyring format`
- `-o /usr/share/keyrings/ubiquiti-archive-keyring.gpg` - Outputs to the system `keyring directory` where APT expects signed repo keys
  
**Recomposition**
Downloads the Ubiquiti official gpg signing key as ascii, converts it to binary and outputs it to the systems keyring directory that APT checks checks for gpg keys

```

use the gpg key in the ubiquiti-archive keyring that you just added to validate the ubiquiti repo as trustworthy, and then add the repo to apt package managers list of trusted repos. /
```ad-bash
	echo "deb [signed-by=/usr/share/keyrings/ubiquiti-archive-keyring.gpg] https://www.ui.com/downloads/unifi/debian stable ubiquiti" | tee /etc/apt/sources.list.d/ubiquiti-unifi.list
```

Refresh package index and install:

```ad-bash
	apt update
	apt install unifi -y
	
**Code decomposition:**

- `apt update` - Refreshes `package index` (now includes UniFi packages from new repo)
- `apt install unifi -y` - Installs the `UniFi Network Application` package and all dependencies (includes Java runtime)

**Additional Context** The `unifi` package has a `dependency` on MongoDB, but won't try to install it because you already have `mongodb-org` installed. APT's dependency resolver sees the requirement is satisfied.
```


**Step 5: Verify Controller Service**

Check if the controller started successfully:

```ad-bash
	systemctl status unifi
	
**[[Command (Linux)#Definition|command]] decomposition:**

- **`systemctl`** = **system control** - This is the main [[Command (Linux)#Definition|command]] for controlling systemd, which is the init system and service manager for most modern Linux distributions. It manages services, targets, and the overall system state. 
- **`status`** = The sub[[Command (Linux)#Definition|command]] that queries and displays the current state of a service. It shows you whether it's running, stopped, failed, enabled/disabled, recent logs, process info, etc. 
- **`unifi`** = The service name (also called a "unit" in systemd terminology). This refers to the UniFi Controller service, which is Ubiquiti's network management software. **Putting it back together:** "Use the system control tool to check the status of the unifi service"
  
**Additional context:** - **systemctl** is called "system control" because it literally controls your system's services and states. Think of it as the remote control for your system's daemons. 

**Mnemonic**: "system + control = systemctl" - you're controlling system services
```

You should see `active (running)` in green. If you see `activating (start)`, give it 30-60 seconds—the controller takes a moment to initialize its database connection with MongoDB.

**Step 6** Get the ip address of the server you're running the unifi controller on. 
You'll see output like: `inet 192.168.1.50/24 brd 192.168.1.255 scope global eth0`
Your controller's IP is the first number (e.g., `192.168.1.50`).
```ad-bash
	ip addr show eth0 | grep "inet "


**Code decomposition:**
- `ip addr show eth0` - Displays all `network interface configuration` for the `eth0` interface (the container's primary network adapter)
- `| grep "inet "` - Filters output to show only IPv4 addresses (note the space after `inet` to exclude `inet6`)

```

**Step 7** Navigate to the Unifi Controller web ui in your browser at the controller . The default port number is 8443

![[image-17.png]]

*Note: You must HTTPS if TLS is enabled, if you use HTTP you will see an error like 'Bad Request. This combination of host and port requires TLS*
![[image-19.png]] ***

**Step 8** Acknowledge potential security risk warning and continue 

![[image-20.png]]

Continue to the Unifi Web Server setup in the browser\

**Step 9** Name your network server and accept the terms of service. Hit Next
![[image-22.png]]


**Step 10** Create a unifi account

Go to https://account.ui.com/register

Fill out the form and agree to the Terms of Service (Don't check the second checkbox unless you want to get marketing emails from Unifi). Click Create Account

![[image-24.png]]

Verify your email using the 2fa token. Then 
Note your password and return to the controller setup wizard to sign into your new Unifi Account
![[image-23.png]]

Enter the next 2fa code sent to your unifi account's linked email ![[image-25.png]]Then click Create Server in the lower right hand corner (forgot to grab screen)

**Step 10** Familiarize yourself with the unifi controller web admin interface
![[image-26.png]]

**Step 11** Adopt the UXG-Lite Router to the controller

1. Plug in the UXG-Lite usb-c power cable into the back of the modem and connect it to a surge-protected outlet. 
   
2. Connect UXG-Lite `LAN` port (icon '<- - ->' to the deco tp-link AP LAN port (same L2 segment as controller). Leave the WAN port disconnected for now. It will be plugged into the ISP modem once adoption is completed. 
3. Adopt the device through the unifi controller web interface. You should see the device ready to adopt in the Unifi Devices tab 
	![[image-27.png]]

4. The device will show 'Adopting' under status. May take several minutes to adopt. If it fails to adopt, note the ip address. If your subnet is different than 192.168.1.x/24, you need to temporarily add that subnet to your lxc container's NIC. 
	1. Navigate to the container console in proxmox, or SSH in. 
	2. check what your net adapter is
	```ad-bash
	ip link show
			
![[image-33.png]]
	```
	3. add a new ip address in the 192.168.1.x/24 subnet 
		```ad-bash
			ip addr add 192.168.1.2/24 dev eth0
		```
	![[image-28.png]]
	4. SSH into the uxg-gateway's cli 
```ad-bash 
	ssh root@192.168.1.1
	username: root
	password: ui
```

5. Run set-inform with your containers primary ip and port to point the uxg-lite to the controller url
```ad-bash
	set-inform http://172.16.0.134:8080 
```
***

6. Return to the controller web-ui and click Adopt. If this worked, the controller will adopt successfully now. 

```ad-img
![[image-27.png]]
```

7. Check status after adoption. If it says 'Update Available' click to update. 
```ad-note
**The update may appear to cancel itself with the status bar dissappearing and the 'Update' button returning. This seems to be a visual glitch, wait a few minutes and the status bar should return. If it doesn't return after a few minutes, click 'Update' again to retry**
```



#### ***Troubleshooting***
###### ***Error Screenshot***

```ad-error 
![[image-14.png]]# paste image here
```

##### Where encountered: 
- Trying to install debian container

##### Why it occurred
- What the root cause of the error was
##### Fix 
- Error fix

##### Troubleshooting resources
1. [Additional Resource][1]

[1]: www.resource.com

#### **Error:** Connection Interrupted During Unifi Device Adoption 

##### ***Error Screenshot***

```ad-error 
![[image-32.png]]
```



##### What it means
- The routers IP address is on a different subnet - the controller can route TO it, but it has no route BACK to the controller. 
##### What I was doing when I encountered it
- **<action1\>**
- **<action2\>**
##### Why it occurred
- **<explanation in context of action1\>**
- **<explanation in context of action2\>**
##### Fix 
- **Started `WSL` session and retried [[Command -- Bash#Definition|command]]**
##### More Info
- [Source][1]
- [Source][2]

[1]: www.source.com
[2]: www.source.com
##### **Error keywords to look for**
- `Frozen runpy` 
- `check_block_io()` 
- `OSError`
- `[WinError 1] incorrect function`
##### ***Error Screenshot***

```ad-error 
![[image.png]]
```






