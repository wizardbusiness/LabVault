![[Discovering Apps - Truenas Scale App Catalog#Steps|Embedded ->App Discovery - Truenas Catalog]]

### 4. \[Section] Choose Application Name and Version
- **Name: Keep default name if this is the only server you'll be using, or choose a descriptive name if you will have multiple servers running on this service**
- **Version Choose the latest version unless you have a reason not to.**
```ad-img
title: More Details
collapse:true
 ![[Backup and Data Storage/Restic/static/image-5.png|720x354]]
```
### 5. \[Section] Restic REST Server Configuration 
---
1. **Timezone: *Choose your Timezone** - *needed for log timestamps*
   ```ad-img
title: More Details
collapse:true
  ![[image-7.png]]
   ```

2. **Append Only \[Optional\]** - *When enabled, this prevents deletion of snapshots through the REST API.*
   ```ad-img
title: More Details
collapse:true

	![[image-9.png|Append-only backup config option]]
	**Additional Thoughts**
	- *Makes your `repository` become write-only.* 
	- *You can't manually delete backups if you chooset this option.* 
	- *Use if you're paranoid about ransomware or admin mistakes deleting your backups.*
   ```

3. **Group Accessible Repositories \[Optional\]: Allows group-level read access to repos.**
   ```ad-img
title: More Details
collapse:true
	![[image-10.png]]
	
	**Additional Thoughts**
	- *Useful for enterprise business domains with identity management.* 
	- *You probably don't need this if you only plan to have a few users.*
   ```

4. **Private Repositories [Optional]**  - *Each user gets their own isolated `repository` space.* 
   ```ad-img
title: More Details
collapse:true
	![[Backup and Data Storage/Restic/static/image-11.png|Restic private repo config]]

	**Additional Thoughts**
	- Enable if different users/systems are backing up to this server.*
   ```

5. **No Authentication \[NOT RECOMMENDED]** - *Disables `HTTP Basic Auth`.* 
   ```ad-img
title: More Details
collapse:true
	![[image-12.png| Enable no auth on http requests to restic server]]
	**Additional Thoughts**
	- *Don't do this unless you're running this on an isolated VLAN or hate security.*
	- *Without HTTP Authentication any device with access to your network gets full read write delete access on your backup server.*
   ```
   
6. **Proxy Authentication Username \[Optional]** *Field to enter Proxy username for Restic to authenticate via proxy server.* 
   ```ad-img
title: More Details
collapse:true
	![[image-13.png|Add proxy auth username to restic server]]
	**Additional Thoughts**

	- *Usually only useful in enterprise networks.*
	- *A proxy is something you deliberately introduce when you need control over what's being communicated, not just where it's going.*
   ```

7. **Basic Auth Users** - *Click "Add" to create a username/password pair for `HTTP Basic Authentication`. You'll need at least one unless you enabled "No Authentication" like a psycho. These credentials are completely standalone and distinct from any truenas-managed credentials.*
   ```ad-img
title: More Details
collapse:true
	![[image-14.png|Basic auth user config example.]]
	**Additional Thoughts**
	- *Basic Auth credentials are sent along with any requests to the server from restic clients.* 
	- *Pretty much everything, from initing a repository to creating a new backup will require these credentials.* 
   ```

8.  **\[Optional] Extra Options** - *This field accepts additional `command-line flags` that provide additional configuration options that are passed to the server when the app docker container initializes*
   ```ad-img
title: More Details
collapse:true
![[image-15.png|Basic options config field]]
   ```
   
   ```ad-example
collapse: true
title: All 'Extra Options' Flags 

***List of Restic Rest Server Option Flags***
*Current as of version 0.14.0*
- `--append-only` - Enable append only mode
- `--cpu-profile string` - Write CPU profile to file
- `--debug` - Output debug messages
- `--group-accessible-repos` - Let filesystem group be able to access repo files
- `-h, --help` - Help for rest-server
- `--htpasswd-file string` - Location of .htpasswd file (default: `<data directory>/.htpasswd`)
- `--listen string` - Listen address (default `:8000`)
- `--log filename` - Write HTTP requests in combined log format to specified filename (use `-` for stdout)
- `--max-size int` - The maximum size of the repository in bytes
- `--no-auth` - Disable authentication
- `--no-verify-upload` - Do not verify integrity of uploaded data ⚠️ only for very low-power devices
- `--path string` - Data directory (default `/tmp/restic`)
- `--private-repos` - Users can only access their private repo
- `--prometheus` - Enable Prometheus metrics
- `--prometheus-no-auth` - Disable auth for Prometheus `/metrics` endpoint
- `--proxy-auth-username string` - Specifies the HTTP header containing the username for proxy-based authentication
- `--tls` - Turn on TLS support
- `--tls-cert string` - TLS certificate path
- `--tls-key string` - TLS key path
- `--tls-min-ver string` - TLS minimum version, one of `1.2` or `1.3` (default `1.2`)
- `-v, --version` - Version for rest-server
  
**Additional Thoughts**
- *These options are passed to the `rest-server` binary whenever the container is initializesd, not just on first install.*
- *The flag set maps conceptually to a 'security vs. convenience dial'.* 
  - *`--no-auth` and `--append-only` sit at opposite ends — one strips all access control, the other adds a hard enforcement layer on top of it.*
- *GUI config entries for flags take precedence over 'Extra Options' flags* 
- *The reason there's a lot of overlap between extra options flags and gui config options is that the app config form in Truenas Scale is just wrapping the original Restic rest server cli config options and exposing the most important/useful config options as GUI entries.*
   ```

9. **\[SKIP] Additional Environment Variables** - *Key-value pairs injected into the container's runtime environment. Doesn't expose any additional options for the restic rest server configuration.*  
   ```ad-warning
**Even though we're skipping this, its worth mentioning - Never expose any sensitive information in environmental variables. Variables are readable as plaintext for anyone with access to the container host via the command `sudo exec -lt <containerid> env`**
   ```

### 6. \[Section] User and Group Configuration
- **User ID** + **Group ID** -  *Both* `568`

```ad-img
title: More Details
collapse: true
   ![[image-19.png]]
   
**Additional Thoughts**	
- *Filled out by default*
- *Truenas builtin 'Apps' User + Group is associated with application permissions by default. Leave this as-is unless you have a reason to change it.* 
	- **UID** `568` 
	- **GID** `568`     
```

### 7. \[Section] Storage Configuration

