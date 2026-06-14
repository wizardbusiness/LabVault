On Windows Server, use OpenSSH

##### Copying over ssh using SCP 
	- SCP Secure Copy Command

On Linux: 

Copy Folder AND contents to new folder in /destination 
```ad-bash
	scp -r /path/to/local/folder username@<destinationip>:/<DriverLetter>:/path/to/destination/

```

Copy folder contents ONLY directly into /destination
```ad-bash
title: bash
	scp -r /path/to/local/folder/ username@<destinationip>:/<DriverLetter>:/path/to/destination/

```
**
Use SCP for**
- Quick one-off transfers of files/folders
- You want the simplest syntax and fastest setup
- You're scripting simple backups or deployments
- You don't need to resume interrupted transfers
- You're moving data between Unix-like systems
##### Copying over ssh using SFTP 
- SFTP `Secure File Transfer Protocol`

```ad-bash
	# Interactive session
	sftp user@host
	sftp> ls
	sftp> cd /path
	sftp> put -r localfolder
	sftp> get remotefile
	
	# Or batch mode
	sftp -b commands.txt user@host

```

**Use SFTP when**
- You need an interactive session
- You want to be able to resume a transfer if its interrupted
- you're using an unreliable connection (see prev)
- you need better filesystem admin capabilities (rename, delete, mkdir, etc)
- automation tools you're using require programmatic access
- You're working with windows a lot

| **Feature Comparison**       | **SCP**          | **SFTP**           |
| ---------------------------- | ---------------- | ------------------ |
| ***Speed***                  | Slightly Faster  | Slightly slower    |
| ***Resume Failed Transfer*** | No               | Yes                |
| ***Directory Browsing***     | No               | Yes                |
| ***File Management***        | No               | Yes(rm, mv, mkdir) |
| ***Error Handling***         | Basic            | Comprehensive      |
| ***Windows Compatibility***  | Worky but Quirky | Worky and Perky    |
