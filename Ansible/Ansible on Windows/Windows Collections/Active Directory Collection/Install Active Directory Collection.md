##### ***Use To*** Install the Ansible Active Directory collection so you can manage the Active Directory on windows hosts

#### Prerequisites
- [[<Internal Link Here>]]
#### More Resources
1. [Ansible microst.ad collection - Ansible Galaxy][1]

[1]: https://galaxy.ansible.com/ui/repo/published/microsoft/ad/?extIdCarryOver=true&sc_cid=RHCTG0180000371695
#### Related Docs
- [[<Related Doc>]]
### *Info*
##### Collection Name: `microsoft.ad`
##### Ansible collection for Active Directory management<sup>[1]</sup>
****
### Instructions
#### 1) Install the `microsoft.ad` collection via `ansible-galaxy`.
```ad-code
title: `code` snippet
collapse: true
	ansible-galaxy collection install microsoft.ad
```
##### *Context*
###### Code Snippet Decomposition

**Syntax**
- `ansible-galaxy` **Parent command- cli tool included with `ansible` for finding community roles and collections** 
- `collection` **Subcommand.  Specifies that you are looking for a collection within the output of `ansible-galaxy`**
- `install`**<action\> to be performed on the collection being targeted**
- `microsoft.ad` **<target\> of the [[Command -- Bash#Definition|command]], the collection that will be installed**

**Synthesis**
1. `ansible-galaxy` + `collection` **<parent [[Command -- Bash#Definition|command]]\> + <sub[[Command -- Bash#Definition|command]]\>; `collection` modifies the behaviour of the parent [[Command -- Bash#Definition|command]] `ansible-galaxy` to act on collections specifically**
2. `install` + `microsoft.ad` **<action\> + <target\>; Combines with the above compound-[[Command -- Bash#Definition|command]] to install the `microsoft.ad` collection**
3. `ansible-galaxy collection install micorosft.ad` **<parent [[Command -- Bash#Definition|command]]\> + <sub[[Command -- Bash#Definition|command]]\> + <action\> + <target\>; finds the micorosoft.ad collection in the ansible galaxy community repository and installs it.**
###### Technical terms and definitions
- *`Ansible Galaxy`:* ***Included with Ansible core installation. A public repository and command-line tool maintained by Redhat/Ansible. Used for sharing, discovering, and installing pre-built Ansible roles and collections that automate common IT tasks.***
- *`Collection`:* ***A standardized distribution format that bundles together related Ansible content including playbooks, roles, modules, and plugins into a single, versioned package for easier sharing and management. Can be downloaded via the `ansible-galaxy` cli tool.***
- *`Built-in Collections`:* **Collections that come included with Ansible***
- `Community Collections`:  ***Collections maintained by the community or third-parties***
- *`Active Directory`:* ***A Microsoft directory service that manages and authenticates users, computers, and other resources within a Windows network, providing centralized security and administration.***
###### Issues Encountered
- **Tried to run ansible-galaxy, got `frozen-runpy`, `incorrect function`,  error.**
	- *Forgot to start WSL session. See Error Definition - [[Frozen Runpy - WinError 1]]*
###### Techniques used in this step
- **Used `ansible-galaxy` cli tool to download the `microsoft.ad` collection**
###### Key concepts that give important context for understanding whats being done in this step
- **`WSL` allows UNIX-based applications like Ansible to work on windows**
- **`CLI Tools` are applications that can be invoked with arguments from the [[Command -- Bash#Definition|command]] Line to accomplish tasks**
- **`Repositories `like Ansible Galaxy are online platforms where people can upload, share and obtain useful resources and tools for software development, operations, automation, and system administration. Think GitHub for sourcecode version control and distribution, npm registry for Javascript pacakges, or PyPI for finding python packages.** 

#### *Troubleshooting*
- ##### Step 1 
	- [[Frozen Runpy - WinError 1|WinError 1]]