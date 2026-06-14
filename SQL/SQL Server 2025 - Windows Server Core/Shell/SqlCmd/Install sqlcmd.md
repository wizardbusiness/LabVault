##### *Why to install?* Lets you type and execute SQL queries into the command line without writing a bunch of ps boilerplate for each query. 

```ad-note
collapse:none
*As of **2022** Microsoft no longer bundles **sqlcmd** with **SQL Server**.*
```
### Resources
- [README go-sqlcmd Github Repo](https://github.com/microsoft/go-sqlcmd#readme)
- [Latest go-sqlcmd version](https://aka.ms/go-sqlcmd)

#### 1. Download the sqlcmd tool from source

*Note: Use the **go-sqlcmd Github Repo** in **Resources** section*

```ad-pscode
collapse: true
	Invoke-WebRequest "https://aka.ms/go-sqlcmd" -Outfile "$ENV:TEMP\go-sqlcmd.msi" # or another folder
```

#### 2. Install the sqlcmd msi package you downloaded

```ad-pscode
collapse: true
	msiexec /i "$ENV:TEMP\go-sqlcmd.msi" /Q /quiet /qn
```

#### 3. Get the updated system path for the session
```ad-pscode
collapse: true
	$env:Path = [System.Environment]::GetEnvironmentVariable("Path", "Machine")
```

#### 4. Test the connection

```ad-attention
*If you are using sqlcmd on a **local domain environment** such as a homelab you'll need to **disable or bypass** certificate validation each time you start a **sqlcmd** session until you trust the sqlcmd **certificate** *
```

```ad-pscode
collapse: true
	# A. For CA Certificate Trusted domains
	sqlcmd -S localhost -E
	#-------------------------------------------
	# B. For local domains such as a homelab
	
	# B1) Trust server certificate ('C') with encrypted connection ('N')
	sqlcmd -S localhost -E -N -C
	
	# B2) Disable certificate validation 
	sqlcmd -S loclahost -E -C
```

#### 5. Trust the SQL Server certificate (Optional but recommended)
 ![[Trust SQL Server Certificate]]
