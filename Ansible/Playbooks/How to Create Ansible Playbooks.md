##### ***Use To***  Learn how to make an ansible playbook

#### Prerequisites
- [[Create Ansible Inventory]]
#### Additional References
1.  [Playbook Definition Check - Claude][1]
2.  [Creating a Playbook - Ansible Docs][2] 

[1]: https://claude.ai/share/e636f7e7-a038-45c1-8cab-246969b9c645
[2]: https://docs.ansible.com/projects/ansible/latest/getting_started/get_started_playbook.html
#### See Also
- [[About Ansible Playbooks]]
### *Info*
##### Ansible playbooks automate the manual work of installing softrware and managing configurations. They target hosts defined in Ansible inventories, and automate the installation and configuration of software, services, and policies on those hosts. For example, in an on-premise Windows domain environment, Ansible can be used to configure a new Domain Controller with the Active Directory Domain Services Role installed and promoted, DNS Zones configured, Chrome web browser installed, and windows update scheduled to run once a day at midnight. <sup>[1]</sup>

***
### Procedure for creating a basic playbook - Demonstration pinging a group of windows hosts 

#### 1. Create a new .`playbook.yaml` file in the same folder as your Ansible inventory
```ad-pscode
collapse: true
	new-item -Path "<ansible project directory>\playbook.yaml"
```

#### 2. Add a name for the playbook
```ad-yaml
collapse: true
	- name: My first play
		hosts: windows

```

#### 3. Step <instruction\>
##### Code Example(s)
```ad-yaml
collapse: true
	tasks:
	   - name: Ping my hosts
     ansible.builtin.ping:
     
	- name: Print message
     ansible.builtin.debug:
       msg: Hello world
```
##### Notes on what's being done
- ###### Note
- ###### Note
- ###### Note
##### Technical terms 
- *Term:* **Explanation**
- *Term:* **Explanation**
- *Term:* **Explanation**
- *Term:* **Explanation**
- *Term:* **Explanation**
##### Questions I'm having while doing this for the first time
- ###### Question 
- ###### Question 
- ###### Question 
##### Example Decomposition
###### **1. Syntax**
- `<code>` **<explanation of what it does/>**
- `<code>` **<explanation of what it does />**
- `<code>` **<explanation of what it does/>**
###### **2. Synthesis**
- `<code>` + `<code>` **<explanation of what it does/>**
- `<code>` + `<code>` **<explanation of what it does/>**
##### Techniques used in this step
- ###### Using x to do y
- ###### Using x to do y
- ###### Using x to do y
##### Key concepts that give important context for understanding whats going on in this step
- ###### Concept
- ###### Concept
- ###### Concept











