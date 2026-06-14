##### ***Use To***  Learn the basics of adding windows hosts to Ansible Inventories

#### Prerequisites
- [[Installing Ansible|An Ansible Control Node]]
- [[Create Ansible Inventory|An Ansible Inventory]]
- [[Configure a Windows Host for Ansible|A Configured Windows Host]]
#### Additional References
1. [Creating Windows Server Inventory for Ansible Automation][1]
2. [Using Ansible to manage windows hosts - GeeksForGeeks][2]
3.  [Getting Started With Ansible > Buidling an Inventory - Ansible Docs][3]

[1]: https://knowitlikepro.com/creating-windows-server-inventory-for-ansible-automation/
[2]: https://www.geeksforgeeks.org/devops/ansible-to-manage-windows-hosts/
[3]: https://docs.ansible.com/projects/ansible/latest/getting_started/get_started_inventory.html#get-started-inventory

#### See Also
- [[Get Started with Ansible]]
### *Info*
##### This document gives some methods specifically for adding windows hosts to the inventory using a beginner-friendly Basic Auth method<sup>[1] [2]</sup>,  a production-ready method with Kerberos for on-premise windows domains, and a more generalized method using Ansible Vaults. 

##### *Additional Thoughts:* Ansible's Windows modules typically use WinRM (Windows Remote  Management) to establish connections from the control node to hosts. This differs from ansible linux hosts which use OpenSSH exclusively. This caused me some initial confusion while reading Ansible's beginner documentation on inventories, since the examples are for linux hosts<sup>[1]</sup> Additionally the various tutorials I found online all had issues, either incorrect code examples<sup>[2]</sup>, or were missing key context like  the fact that Ansible using HTTPS for WinRM connections by default. This documentation draws on them for their good parts and improves upon them with additional context and working code for my specific usecase of using Ansible in my Homelab.

***
![[Add a Host to Ansible Inventory#Adding a Windows Host Configured with WinRM and Basic Auth]]

