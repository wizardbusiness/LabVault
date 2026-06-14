##### ***Use To*** Generate an ssh key pair for use with Ansible inventory and add it to each host so that Ansible can communicate with them. 

#### Prerequisites
- [[Installing Ansible|Ansible Installed]]
#### More Resources
1. [Ansible SSH Overview & Setup - LLTV][1]
2. [Securing SSH private keyrs without a passphrase][2]

[1]: https://www.youtube.com/watch?v=-Q4T9wLsvOQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70&index=2
[2]: https://claude.ai/share/e63f1053-fa84-4ff9-b348-691e4bf299b5
#### Related Docs
- [[Create Ansible Inventory]]
### *Info*
∙	**Ansible is agentless - it requires no software installed on target machines. It connects over SSH to execute work remotely**
∙	Authentication follows standard SSH conventions — key-based authentication is the norm; password auth works but is discouraged.
∙	**On the control node (where you control Ansible hosts from) Ansible respects your existing SSH config (~/.ssh/config) and uses your SSH agent if running.**

****
### 1. Generate a new SSH key on the Ansible host
***Note: You can also use an existing ssh key, but having one specifcially for ansible keeps things cleaner.***
```ad-bash
~~~bash
ssh-keygen -t ed25519 -C "ansible"
~~~

**Decomposition**
- `ssh-keygen` - Builtin tool that generates an SSH key. 
- `-t` - Flag that stands for 'type' when appended to the `ssh-keygen` command.
- `ed25519` - modern ssh keygen hash algorithm that is more secure than using the default.
- `-C` - Flag that stands for 'Comment'
- `"ansible"` - Comment text in string quotes, in this case "ansible"

  
  

**Recomposition**
- `ssh-keygen` + `-t` + `ed25519` + `C` + `ansible` - Generates a public/private ssh keypair using the `ed25519` secure hashing algorithm, and comment it with "ansible" for documentation purposes. 
```

### 2. Secure your SSH Key 
***Note: This will generate 2 files - `~/.ssh/ansible` and `~/.ssh/ansible.pub`, respectively containing your private and public keys.***

```ad-danger
title: Warning
**If you do not specify a path within the `~/.ssh` directory when prompted, your default key `~/.ssh/id_ed25519` will be overwritten**
```

#### **Handling programmatic access to your Ansible Hosts via SSH.**
#### **A)** Easiest and Least-Secure Method - Don't set a password on your SSH Key

```ad-info
**This is the method you'll typically see in intro to ansible tutorials. The reason you don't set a password on the private key file is that Ansible needs to be able to communicate with it automatically, and Ansible doesn't handle credentials by default.**

**Pros**
- Easy to set up, lowers the amount of friction getting you up and running with Ansible.
- Easy to use, you can effortlessly SSH into any host with the public key, no additional typing required.

**Cons**
- The private key file is accessible locally to anyone with file-level permissions to the key. 
- Better safe than sorry. If someone get's that private key who isn't supposed to you're absolutely fucked, they have access to all your hosts now. 
```

##### **1.** Save the new SSH keypair to the `~/.ssh/ansible` path when prompted. 
##### **2.** Press `Enter` twice on the next prompt to continue without setting a password.

---
#### **B)** Slightly harder, much more secure method - Set a password on your SSH Key and give to `SSH-Agent` to handle  

```ad-info
**This method allows you to set a password on your key, making it much more secure. An SSH-Agent brings up a prompt to enter the password at the start of your shell session. After your enter the password it's hashed and automatically applied to any ssh connection attempts using that ssh key. Ansible can still access target hosts automatically as it's not being interrupted by password prompts.**

**Pros**
- Relatively easy to set up, dramatically improves ssh security.
- Password only needs to be entered once by the user at the beginning of each session, allowing Ansible to access hosts programatically.  
  
**Cons**
- Slighlty more friction than just not setting a password. Annoying when you're just trying to get started learning Ansible
- You still have to manually enter the password at the beginning of each session. 
```


```
***Example of the command prompt readout after executing the command.***
```txt
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/gabriel/.ssh/id_ed25519): <enter-path-here>
Enter passphrase for "/home/<user>/.ssh/<entered-keyname-here>" (empty for no passphrase): 
Enter same passphrase again:
Your identification has been saved in /home/<user>/.ssh/<entered-path-here>
Your public key has been saved in /home/<user>/.ssh/<entered-keyname-here>.pub
The key fingerprint is:
SHA256:<hash-of-the-public-key> <entered-keyname-here>
The key's randomart image is:
+--[ED25519 256]--+
|*B.++         o*B|
|B.X+ o        ++E|
|.B =*.. ..    =+X|
|. + +=o..o. . + .|
| E + *. S  . o . |
|      o          |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

### 3. Copy the SSH key to each server in your Ansible Inventory
***Look at your ansible inventory file and substitute in each  \<server-ip-address>***
```ad-bash
~~~bash
# Do this for each server
ssh-copy-id -i ~/.ssh/ansible.pub <user>@<server-ip-address>
~~~

##### **Decomposition**
- `ssh-copy-id` - Builtin utility that copies an SSH key to a target network device running open-ssh. 
  
- `-i`  - Flag that stands for 'identity file'. It points the `ssh-copy-id` command to a specified public key. *Note: `ssh-copy-id` defaults to `~/.ssh/id_rsa.pub` public key if `-i <path>` isn't included.* 
  
- `~/.ssh/ansible.pub` - Public key for "ansible" Public-Private Keypair. Must be in the same directory as the matching private key file. 
  
- `<user>` - Placeholder representing the user that the ssh session will authenticate as on the target host. 
- `@` - Delimiter character. Used by the SSH parser to split the `<user>@<server-ip-address` string argument into user and host. 
- `<server-ip-address>` - placeholder representing the ip address of the target host. 

##### **Recomposition**
- `ssh-copy-id` + `-i` + `~/.ssh/ansible.pub` + `<user>` + `@` + `<server-ip-address>` - Copies the ssh public key in the target identity file to the target host using the target user on that hosts' credentials. 
```
