#### **Test connectivity across your whole inventory**

> [!seealso] More Resources
> 1. [Ping Module - Ansible Docs][1]
> 2. [Ansible.cfg reference sheet - Ansible Docs][2]
> 3. [Interpreter Discover - Ansible Docs][3]
> 
> [1]: https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/ping_module.html
> [2]: https://docs.ansible.com/projects/ansible/latest/reference_appendices/config.html
> [3]: https://docs.ansible.com/projects/ansible-core/2.20/reference_appendices/interpreter_discovery.html 
> 


> [!bash] Bash `snippet`
> ~~~bash
> ansible all --key-file ~/.ssh/agent/ -i inventory -m ping
> ~~~
> 
> ---
> > [!learn]- **Learn**
> > > [!decomp]- *Decomposition*
> > > - `ansible` - [[Ansible#Definition|Ansible]] CLI binary.
> > > - `all` - A built-in [[Ansible#Definition|ansible]] [[Command -- Ansible#Definition|command]] that targets every host defined in your inventory. 
> > > - `--key-file` - Specifies the private SSH key Ansible will use to authenticate into the target hosts. 
> > > - `~/.ssh/<ansible-key>/` - Path to the private key you've configured to use for Ansible authentication. 
> > > - `-i` Short for `--inventory`. Tells Ansible where to find the list of hosts to target.
> > > - `inventory`  - The inventory file in your current working directory. This file defines your Ansible hosts and groups.
> > > - `-m` - Short for `--module-name`. Ansible operates via modules. 
> > > - `ping` - the Ansible `ping` module. Different from regular ping. Does a full round-trip: SSH into the host, verify Python is available, return `pong` if successful. It verifies both connectivity and that host can run Ansible commands .
> >
> > >[!recomp]-  *Recomposition*
> > >- `keyword` + `keyword` + `keyword` - Definition
> > 
> > > [!output]- *Output*
> > > ![[image.png]]
>> ---
> > > [!seealso] **About Ansible Ping**
> > > - *`ping` verifies that python is installed because Ansible modules use python scripts to run - Without python Ansible can't do very much.*
> > > - *`ping` is NOT the ICMP network tool ping - It is Ansible's own [built-in module][1]*.
> > > - *The warning about the Python interpreter (see output example) is just a heads up that Ansible defaults to the first-found python interpereter. This becomes important if  a module has a dependency that requires a certain version of python to be used.*
> > > 	- *You can [configure][3] Ansible's python interpreter via the config file - [`ansible.cfg`][2]*
> 




