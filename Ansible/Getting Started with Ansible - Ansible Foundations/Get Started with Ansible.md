## Automation with Ansible
##### ***Use This Doc To
- ##### Familiarize yourself with the basics of using Ansible on Windows
- ##### Understand the limitations of Ansible in Windows. 
#### Prerequisites
- [[<Internal Link Here>]]
- [\<External Link Here\>]()
#### Additional References
1. [Managing Windows Hosts with Ansible - Ansible Docs][1]
2. [Using Windows as a control node - Ansible Docs][2]
3. [How to install software with Ansible][3]
4.  [Ansible Module Index][4]

[1]: https://docs.ansible.com/projects/ansible/latest/os_guide/intro_windows.html#managing-windows-hosts-with-ansible
[2]: https://docs.ansible.com/projects/ansible/latest/os_guide/intro_windows.html
[3]: https://opensource.com/article/20/9/install-packages-ansible
[4]: https://docs.ansible.com/collections.html


****
## About Ansible on Windows

##### Ansible is built to run on linux, but includes a comprehensive suite of module for Windows-based deployments. You can use Ansible to write playbooks for windows devices, and even have an Ansible control node running in windows via WSL. However Ansible control nodes in WSL are not officially supported by Ansible, and thus should should not be used in production environments.  

```ad-important
>**""The Windows Subsystem for Linux is not supported by Ansible and should not be used for production systems.""^[https://docs.ansible.com/projects/ansible/latest/os_guide/intro_windows.html]**
```

## Introduction to Ansible
### **Inventory** 
### **Playbooks**

##### Playbooks are Ansible's answer to automating the setup and configuration of pretty much any windows or linux pc.  They can be used to automate anything from installing software, to configuring users, to setting the operating system update schedule. They are written in YAML for simplicity and contain step-by-step instructions specifying exactly what to do when setting up a device. Playbooks use *actions* which define what a device should . In Ansible each instruction step is called a module. Modules are either included in Ansible by default as *core modules*, or written by the community, and can be used to accomplish pretty much any configuration task.  Behind the scenes, modules are really scripts that execute and accomplish a task when they are included in a playbook. Here is an example of a playbook that contains modules for configuring a new Windows 11 Computer with a dynamic IP, DHCP server, and update schedule. 

### **WSL**

## ___ Use: Example

```ad-pscode
collapse: true
	
```

### **Modules**

##### Ansible uses modules to do stuff. In order to accomplish complex multi-step configuration, modules are grouped together in **playbooks**

#### **Ansible Core Modules on Windows**

##### Below is a list of Ansible core modules which work on windows. Note that the majority of modules are written for a Unix environment and will not work on Windows. 
