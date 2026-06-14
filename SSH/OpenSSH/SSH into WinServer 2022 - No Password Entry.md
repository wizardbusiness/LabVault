#### 1. Get the content of your public key

```ad-bash
collapse: true

	cat ~/.ssh/id_ed25519.pub
```

#### 2. SSH into Windows Server 

```ad-bash
collapse: true
	ssh <username>@<windows-server-ip-address> 
```

#### 3. Echo the content of the public key into a new ASCII file *administrators_authorized_keys* in  *C:\ProgramData\ssh*

```ad-pscode
collapse: true
	Echo "ssh-ed25519 AAAA<rest-of-key-here> <associated-email>@<domain>.com" | Out-File -Encoding ASCII "C:\ProgramData\ssh\admnistrators_authorized_keys"
```

#### 4. Set permissions to restrict access 

```ad-pscode
collapse: true

	# Remove inherited permissions and set strict access 
	icacls C:\ProgramData\ssh\administrators_authorized_keys /inheritance:r 
	icacls C:\ProgramData\ssh\administrators_authorized_keys /grant "BUILTIN\Administrators:F" 
	icacls C:\ProgramData\ssh\administrators_authorized_keys /grant "NT AUTHORITY\SYSTEM:F"
```

#### 5. Restart the ssh server service

```ad-pscode
collapse: true
	Restart-Service sshd
```

#### 2. Test SSH working correctly

```ad-bash
collapse: true
	# Should ssh you in without needing to enter a password
	ssh <username>@<windows-server-ip-address> 
```

```ad-bash
	Hello world
```


