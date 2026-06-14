
#### 1. Open the linux .smbcredentials file
```ad-bash

	nano /root/.smbcredentials
	
Breaking it down:
- `nano` - Text editor (or use `vi`, `vim`, whatever you prefer)
- `/root/.smbcredentials` - Hidden file in root's home directory
- `.` prefix makes it hidden from normal `ls` output


```

#### 2. Add these lines 
```ad-bash
	username=your-username
	password=your-password

- `username=` - SMB/CIFS username on TrueNAS (no quotes needed)
- `password=` - The password for that user (no quotes needed)
- No spaces around the `=`
- Each entry on its own line
```

#### 3. `Ctrl` + `o` to save file, `Ctrl` + `x` to exit editor
