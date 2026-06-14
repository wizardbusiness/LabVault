
### 1. Enter the following [[Command -- Bash#Definition|command]]

```ad-bash
~~~bash
ssh-keygen -p -f ~/.ssh/<keyname>
~~~

**Decomposition**
- `ssh-keygen` - the key generation and management tool from the OpenSSH suite. Despite the name, it does more than just *generate* keys - it also inspects, converts, and modifies them.

- `-p` - stands for ** [[Passphrase.md#Definition|Passphrase]]**. Tells `ssh-keygen` you want to change (or add) a  [[Passphrase.md#Definition|Passphrase]] on an existing key rather than generate a new one.

- `-f` - Stands for 'file'. 
- `~/.ssh/<keyname>` - Points to the private key file you want to modify. Swap `<keyname>` for whatever your key is named (`id_rsa`, `id_ed25519`, etc.).

**Recomposition**
- `ssh-keygen` + `-p` + `-f` + `~/.ssh/<keyname>` - Uses the `ssh-keygen` tool to add or modify 
  
```

### 2. Add, Change or Remove a  [[Passphrase -SSH-#Definition|Passphrase]] on the private key

#### A) Add a  [[Passphrase -SSH-#Definition|Passphrase]] 
**Enter a new  [[Passphrase -SSH-#Definition|Passphrase]] when prompted**

#### B) Change an existing  [[Passphrase -SSH-#Definition|Passphrase]].
1. **Enter the existing  [[Passphrase -SSH-#Definition|Passphrase]]** 
2. **Enter a new  [[Passphrase -SSH-#Definition|Passphrase]]** 
3. **Enter the new  [[Passphrase -SSH-#Definition|Passphrase]] again.**

#### C) Remove an  existing  [[Passphrase -SSH-#Definition|Passphrase]] 
1. **Enter the existing  [[Passphrase -SSH-#Definition|Passphrase]]** 
2. **press `Enter` without entering a new  [[Passphrase -SSH-#Definition|Passphrase]]**

**After successfully adding, changing or removing a  [[Passphrase -SSH-#Definition|Passphrase]] you will see the message -**  
```txt
Your identification has been saved with the new  [[Passphrase.md#Definition|Passphrase]].
```

#### **Reasons to set a  [[Passphrase -SSH-#Definition|Passphrase]] on a private key.**
- **A  [[Passphrase -SSH-#Definition|Passphrase]] encrypts a private key file at rest. Even if someone gets the private-key file, they still can't use it without the  [[Passphrase -SSH-#Definition|Passphrase]].**
- **Without a  [[Passphrase -SSH-#Definition|Passphrase]], your private key is just a plaintext file.**
- **Yes, typing a password is annoying, but this is why `ssh-agent` exists: you unlock the key once with the  [[Passphrase -SSH-#Definition|Passphrase]], and the agent holds it in memory so you don't have to type it every time. See -> **