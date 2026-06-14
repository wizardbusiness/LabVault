
#### 1. Check if Open SSH client and server capabilities are installed
```ad-pscode
collapse:true
	Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
	
	# Check if installed ->
		# OpenSSH.Server~~~~0.0.1.0 
		# OpenSSH.Client~~~~0.0.1.0
```

#### 2. Add Open SSH Capabilities
```ad-pscode
collapse:true
	Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
	# repeat for Client if needed, but it usually comes preinstalled
```

#### 3. Start the service. Set it to start automatically on boot.** 
```ad-pscode
collapse:true
	Start-Service sshd
	Set-Service -Name sshd -StartupType 'Automatic'
```

#### 4. Enable SSH firewall rules
```ad-pscode
collapse:true
	Get-NetFirewallRule -Name *ssh* | Where-Object {$_.Enabled -eq 'False'} | Enable-NetFirewallRule
```

***
#### ***Troubleshooting***

##### References
- [Cant Install OpenSSH Features](https://learn.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/cant-install-openssh-features)
#####  ***a. Error Code 0x8024402C***
```ad-img
![[image-1.png]]
```

##### Steps to Resolve
###### ***1. Make sure that you are connected to the internet. If ping fails see [[Configure Static IPV4 IP Address]]**

```ad-pscode
	ping 8.8.8.8
```

###### ***2. Check if dns resolves correctly. If the address won't resolve see [[Set DNS Server]]***
```ad-pscode
	ping google.com
```

****

