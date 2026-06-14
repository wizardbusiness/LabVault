##### ***Error Code: NewConnectionsError***
##### ***Error Screenshot***

```ad-error 
![[image-1.png]]

```

#### Where encountered: 
- Testing connection for windows hosts in inventory.ini configured to connect via winrm
#### Why it occurred
- I'm using WinRM in the Ansible windows host inventory.
- When WinRM is specified Ansible attempts to connect to the host default WinRM port 5986 which is for reserved for HTTPS
- Using HTTPS on port 5986 requires a certificate, which my host currently doesn't have (its running on my homelab, its fine) 
- Without a certificate my server is only actively listening on HTTP port 5985
- Therefore the connection request to port 5986 was refused
#### Fix 
- Check what winrm ports have active listeners-> Result: Only listening on 5985 with HTTP protocol
```ad-pscode
	winrm enumerate winrm/config/Listener
```
- Specify port 5986 in inventory host vars. 
```ad-img
title: **inventory.ini**
![[image-2.png]]
```

```ad-success
![[image-4.png]]
```
#### Additional Resources
- [Claude Conversation - Ansible Project][1]
[1]: [www.resource.com](https://claude.ai/share/e75f34a2-3a0d-4017-88a0-f91ef9bcb503)
