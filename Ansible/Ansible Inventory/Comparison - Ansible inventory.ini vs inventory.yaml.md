##### ***Use To*** Learn about the similarites, differences and usecases of each filetype

#### See Also
- [[Create Ansible Inventory]]
- [[Add a Host to Ansible Inventory]]
### Choosing `.ini` vs choosing `.yaml` for your Ansible inventory

#### About Inventories
##### Ansible inventories are used to store host machine configurations. They tell Ansible which hosts to connect to, how to connect to them, and how to handle authentication. Inventories can store mulitiple host configurations which can be grouped logically by topology, location, and environment. You can use either `.ini` or `.yaml` to build your Ansible inventory, here's a quick guide to help you decide when to use each. 

#### Using inventory.ini

**Advantages:**
- Minimal Syntax
- Simple flat structure keeps inventory data cleanly organized and readable
- Whitespace insensitive, less likely to have indentation errors
-  Quick to edit and read
- Wide familiarity in IT world (Though YAML is getting more common)
**Disadvantages:**
- Flat structure doesn't visually represent hierarchical data relationships well. Must either
	- escape complex nested structures as strings
	- flatten them and lose visual relationship structure
- Any Inline variables must be defined on a single line.
	- *Note: this is resolved by using host_var definitions, see examples below
- Does not support encryption
##### **Example** Inventory.ini with a single inline `scalar` `host_var`

**A) `inventory.ini` with multiple inline `scalar` `host_vars` for `win_host_1`**
```ad-code
title: `inventory.ini` **Example**
	[windows]
	win_host_1 ansible_host=172.16.0.141
```

##### **Example** Inventory.ini with inline variables vs inventory.ini with a separate` .yaml` definition file for win_host 1

**A) `inventory.ini` with multiple inline `scalar` `host_vars` for `win_host_1`**
```ad-code
title: `inventory.ini` **Example**
	[windows]
	win_host_1 ansible_host=172.16.0.141 server_role=domain_controller dc_site=HQ fsmo_roles="['schema_master', 'domain_naming_master']"
```

**B) `inventory.ini` with multiple inline `vector` `host_vars` for `win_host_1`**

**-VS-**

**C) `inventory.ini` with separate `host_var` definitions for `win_host_1` *

**`inventory.ini` file with `win_host_1` defined**
```ad-code
title: `inventory.ini` **Example**
	[windows]
	win_host_1
```


##### **Putting It All Together** `.ini` inventories have constraints that cause visual clutter and friction when used past a certain scale and complexity, but that also keep them easy to understand and use at smaller scales. A great example of this is the handling of inline variables, which are great for representing  simple scalar values but whichturn to spaghetti when representing  multiple complex data types. If you still want to use a `.ini` inventory Ansible works around this limitation by incorporating support for `host_vars` and `group_vars` `.yaml` to represent complex nested datastructures while keeping the actual `inventory.ini` advantages listed above. 

**`host_vars` for `win_host_1` defined in separate `host_vars/win_host_1.yaml` file** 
```ad-yaml
title: **`host_vars/win_host_1.yaml`example**
	ansible_host: 172.16.0.141
	server_role: domain_controller
	dc_site: HQ
	fsmo_roles:
	  - schema_master
	  - domain_naming_master
```
##### **Rough Guidelines for Use** Use a `.ini` inventory when you have a simple, flat inventory with few inline variable definitions and flat data structures. If your inventory host structure is flat, but your `host_vars`  contain  nested data structures, split the definitions out into separate `<hostname>.yaml` files  inside the specially-named `/host_vars`  directory. and `/group_vars`directories. 

#### Using inventory.yaml

##### Lorem Ipsum

