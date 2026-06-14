#### ***Use This Doc To***  Install windows server 2022 to a Proxmox VM

#### Prerequisites
- [[<Internal Link Here>]]
#### References
-  []()
#### See Also
- [[<Related Doc>]]
****
### *Preface*
##### Context for the instructions here

### Instructions

#### 1. Boot from bootable media containing .iso 

#### 2. Hit 'Next', then 'Install Now'

![[Windows/Windows Server 2022/Installing Windows Server 2022/static/static_Install Windows Server 2022 To Proxmox VM/image.png|697x522]]

#### 3. Choose version to install
##### **a. Standard Edition (Server Core):** 
- Provides the minimal installation with up to 2 VM licenses, 
- No GUI—just command line and PowerShell. Maximal security, minimal attack surface, and lower resource overhead.
- Use this in production environments where the above is important
##### **b. Standard Edition (Desktop Experience):** 
- Provides the full GUI installation with up to 2 VM licenses
- Includes Windows Explorer, MMC consoles, and all graphical management tools. 
- Good for learning, GUI windows admin, or for roles that require graphical applications.

##### **c. Datacenter Edition (Server Core):** 
- Provides the minimal installation with unlimited VM licenses and advanced features like Storage Spaces Direct and Storage Replica. 
- Use this in highly virtualized environments running many VMs per host from windows server, where the unlimited licensing justifies the cost. (note proxmox is cheaper and better for virtualization if your environment allows it.)

##### **d. Datacenter Edition (Desktop Experience):** 
- Provides the full GUI installation with unlimited VM licenses and all Datacenter features. 
- Use for enterprise virtualization hosts where you need advanced features, unlimited virtualization licensing, and prefer local graphical management (rare in production).

```ad-info
**Windows server Eval copies can be used for 180 days (6 months) without purchasing a license**
```

![[image-1.png|713x534]]

#### 5. Accept License agreement, hit 'Next' 

![[image-2.png|713x554]]
#### 6. Choose 'Install Microsoft Server Operating System only (Advanced)'
![[image-3.png|718x558]]
#### 5. Choose drive, hit 'Next'
 ![[image-12.png|729x550]]

```ad-help
title: #### **No drives show up? Click Here**
collapse:true
##### **1. Load Driver**
![[image-4.png]]

##### 2.Browse to virtio driver .iso 
![[image-5.png]]

##### 3. Browse to w11 folder inside the amd64 directory 
![[image-8.png]]

##### 4. Install virtio scsi pass-through controller (or whichever appears as an option for your controller type)
![[image-9.png]]

##### 5. Let the driver install
![[image-10.png]]

	
```

![[image-12.png|729x550]]
#### 6. Let the wizard finish the install
 ![[image-13.png|736x168]]

#### 7. Finishing up
1. **Let the vm reboot**
2. **Change admin password - Must be at least 7 chars and contain at least 1 special character and 1 number**
	![[image-14.png|748x172]]
3. **Select 'OK' and press 'Enter'**
	![[image-15.png|758x173]]

4. **SConfig will load, you can now continue setting up your Server**
	![[image-16.png|770x406]]
#### 8. Done!

