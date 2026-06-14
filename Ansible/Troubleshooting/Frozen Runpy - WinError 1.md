#### **Error:** `OSError: [WinError 1]`

##### What it means
- **Ansible is not supported in the current OS environment**
##### What I was doing when it occurred 
- **Trying to install a collection from the ansible-galaxy cli tool**
- **Trying to list inventories from the ansible cli tool**
##### Why it occurred
- **Ansible on windows needs to run inside WSL and i was trying to run it in a Powershell session**
##### Fix 
- **Started `WSL` session and retried [[Command -- Bash#Definition|command]]**
##### More Info
- [Ansible Installation Error On Windows - Ansible Forums][1]
- [Stack Overflow Thread - Install Ansible Windows Machine][2]

[1]: https://forum.ansible.com/t/ansible-installation-error-on-windows-10/3930/4
[2]: https://stackoverflow.com/questions/74701206/install-ansible-windows-machine
##### **Error keywords to look for**
- `Frozen runpy` 
- `check_block_io()` 
- `OSError`
- `[WinError 1] incorrect function`
##### ***Error Screenshot***

```ad-error 
![[image.png]]
```







