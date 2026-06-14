
```ad-bash
	ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote-host
	
**Decomposition**
- `ssh-copy-id` - Utility that copies your public key to a remote machine's authorized_keys
- `-i ~/.ssh/id_rsa.pub` - Specifies **i**dentity file (the public key to copy)
    - `~/.ssh/id_rsa.pub` - Path to your public key file
- `user@remote-host` - SSH connection string (username @ hostname/IP)
```
