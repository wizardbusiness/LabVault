##### ***Use To*** Be able to use a password on your SSH private key(s), but only have to enter the key once at the start of a session. 

#### Prerequisites
- [[<Internal Link Here>]]
#### More Resources
1. [Setting up SSH key passphrase with ssh-agent - Claude][1]

[1]: https://claude.ai/share/0a87da4e-5b38-4092-9982-aa873abf5e41
#### Related Docs
- [[<Related Doc>]]
### *Info*
##### Lorem Ipsum

****
### 1. Start the SSH agent *Daemon*
**The agent must be run at the start of each session.**
>[!bash] Bash `snippet`
> ~~~bash
eval "$(ssh-agent -s)"
>~~~
>
> > [!example]- **Decomposition**
> > - `eval` - Stands for **'Evaluate'**. Command that evaluates a \<string> argument, and executes it as code within the current session. Can be used to evaluate raw strings **OR** the output of another command. 
> >- `"$(<command>)"` - Command substitution syntax for bash. Captures the output of the command inside `$()` to the surrounding string. *Note: Must use double-quotes (`"` `"`) for command substitution to work* 
> > - `ssh-agent` - A background daemon that holds decrypted private keys in memory. Called an **'Agent'** because it acts _on your behalf_, handling auth challenges without exposing your key.
> >- `-s` - Stands for **'sh syntax'**. Tells `ssh-agent` to format it's output for execution by the *bash* (Bourne) shell specifically. *Note: `ssh-agent` command also supports the c-shell format by substituting `-s` for `-c`*
> 
> 
> > [!tip]- **Recomposition**
> > - `eval` + `"$(<command>)"` - Runs \<command> as a sub-process and returns its output to the current process as a string, then executes it as *bash* code within current shell session. *Note: Eval is dangerous because it can be used to smuggle malicious code into the terminal in string format*
> > - `ssh-agent` + `-s` - Outputs a newline-delimmited string in *bash* shell format containing local environmental variable definitions and export commands
> > - **Example: Output of `ssh-agent -s`**
> > >~~~txt
> > >SSH_AUTH_SOCK=/tmp/ssh-XXXXXX/agent.1234; export SSH_AUTH_SOCK;
> > >SSH_AGENT_PID=1234; export SSH_AGENT_PID;
> > > echo Agent pid 1234;
> > > ~~~
> > > - `SSH_AUTH_SOCK` - Path to Unix socket that - when set - the *ssh* process sends auth challenges to. The *ssh* agent listens on the socket for *ssh-auth challenges* and answers them with the decrypted *ssh key* stored in its memory. *Note: The auth socket is permission-restricted to the current user and root and blocks non-root processes from inspecting it. Root having access is a known security compromise, but if root's been compromised you're probably pretty fucked anyways*  
> > > - `SSH_AGENT_PID` - The process ID of the ssh agent. Used to initiate kill command by agent process when it's finished.
> > > - `echo Agent pid 1234` - Prints the *ssh agent process ID* to the terminal.
> >  - ***Finally*** `eval` + `"$(ssh-agent -s)"` - Runs `ssh-agent -s` as a sub-process and returns its stdout to the current shell session as a delimmited-string formatted for the *bash* shell. Evaluates the string and executes each command inside the current shell session. 
> 
> >  ##### **Additional Thoughts**
> >  - *If you don't want to type `eval "$(ssh-agent -s)"` at the start of each session, you can use systemd to run it automatically on login.*
> >  - *If you close the terminal session / otherwise stop the terminal process the agent daemon will stop. To start using the agent to automtatically handle passphrase auth again just restart the agent with `eval" ($ssh-agent -s)`.*
> 

*- - -  Stop here if you've already added your private key to the agent - - >* 
### 2. Give passphrase for SSH key to the agent. 
***Note: After a key passphrase has been added it does not need to be added again.***
> [!bash] Bash `snippet`
> ~~~bash
> ssh-add ~/.ssh/<key-name>
> ~~~
> 
> > [!example]- **Decomposition**
> > - `ssh-add` - Adds a private key to the running `ssh-agent`. 
> > - `~/.ssh/<key-name>` - path to your private key. Omit the path entirely to add default keys (*id_rsa*, *id_ed25519*, etc.)
>
> >[!tip]- **Recomposition**
> >- `ssh-add` + `~/.ssh/<key-name>` - Adds the private key at the path `~/.ssh/<key-name>` to a running `ssh-agent` daemon. 
> 
> > **Additional Thoughts**
> >- *The SSH-Agent daemon must be running in order to add a key to it.*
> >- *You only need to add a private key's passphrase to the agent **once**. Afterwards the decrypted key is automatically loaded into > the agent's working memory.*
> 
	
	