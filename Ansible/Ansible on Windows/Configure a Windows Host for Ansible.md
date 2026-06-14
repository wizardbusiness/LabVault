##### ***Use To***  Configure a windows host to allow connections from Ansible Control Node

#### Prerequisites
- [[<Internal Link Here>]]
#### Additional Resources
-  [[1] example.com reference alias][1]

[1]: www.example.com
#### See Also
- [[Comparison - Ansible inventory.ini vs inventory.yaml]]
### *Info*
##### Context for the instructions here

***
### Prepare Host Using Basic Authentication + HTTP-Only Windows Remoting
```ad-warning
**Not for production environments. Only use this when first getting familiar with Ansible and in testing and lab environments isolated from production.**
```
#### 1. Enable Powershell Remoting
```ad-pscode
collapse: true
	Enable-PSRemoting -Force
```

#### 2. Allow unencrypted network traffic
```ad-pscode
collapse: true
	Set-Item -Path WSMan:\localhost\Service\AllowUnencrypted -Value True
```

#### 3. Allow Basic Authentication with Username and Password
```ad-pscode
collapse: true
	Set-Item WSMAN:\localhost\Service\Auth\Basic -BasicAuth -Value True
```

#### 4. Enable firewall rule for Remoting via WinRM
```ad-pscode
collapse: true
	Enable-NetFirewallRule -Name "WINRM-HTTP-In-TCP"
```


