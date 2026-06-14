##### ***Use To*** Learn about the purpose, logic, and terminology of ansible playbooks

#### Additional References
1.  [Techworld With Nana > What is Ansible - Youtube][1]

[1]: https://www.youtube.com/watch?v=1id6ERvfozo
#### Related Docs
- [[Create Ansible Inventory]]
- [[How to Create Ansible Playbooks]]
### *About Ansible Playbooks*

- ##### Written in .YAML, which has widespread adoption and human-friendly syntax.
- ##### Automate the manual work of installing softrware and managing configurations. 
- ##### Execute on hosts in Ansible inventories.
- ##### Automate the installation and configuration of software, services, and policies on target hosts. 
#### Playbook Example

- ##### In an on-premise Windows domain environment, Ansible can be used to configure a new Domain Controller with the Active Directory Domain Services Role installed and promoted, DNS Zones configured, Chrome web browser installed, and windows update scheduled to run once a day at midnight. <sup>[1]</sup>

![[Playbook Examples#^8ee78b]]

#### Playbook Structure

##### Playbooks are structured logically in order to accomplish automate sequences of tasks - *Tasks* define *what* will be done, *modules* define *how* it will be done, *hosts* define *where* it will be done, and the *user* defines *which* user will do it. When tasks are grouped together inside a playbook, they are called a *play*.
##### Plays 
- ###### A sequence of tasks is called a play
##### Tasks (the *what*)
- ###### Tasks are defined as one or more modules arranged sequentially that together perform an action. 

```ad-yaml
title: **tasks example**
	tasks: 
	  - name: creatre directory for nginx
		file:
			path: /path/to/nginx/dir
			state: directory
	
	  - name: install nginx latest version 
		yum: 
			name: nginx
			state: latest
		  
	  - name: start nginx
	    service:
			name: nginx
			state: started
```

##### Modules (the *how*)
- ###### Small programs that do the actual work. 
- ###### Each module does one specific task
- ###### Agentless action
	- ###### Get pushed from the control node to the target host and do their work
	- ###### get removed when done.
- ###### Hundreds of modules to accomplish pretty much any IT task you can think of. 

```ad-yaml
title: **module example**
	# excerpt from tasks example
	- name: install nginx latest version 
	    # module
		yum: 
			# module arguments
			name: nginx
			state: latest
```

##### Hosts (the *where*)
- ###### The system(s) on which tasks should be executed
- ###### A host can be any network addressable physical or virtual device running a supported operating system. 
- ###### Also referred to as nodes in the ansible docs
- ###### Can refer to a specific host, or a host *group*

```ad-yaml
title: **Host group example**
	# host group
	- hosts: databases
```
##### User (the *which*)
- ###### Refers  to an authenticated user within the host operating system
- ###### Ansible authenticates as the user in order to accomplish tasks
- ###### A user must be an Administrator with elevated privileges in order to work with ansible. 

```ad-yaml
	- hosts: databases
	  # user
	  - remote_user: root
```


