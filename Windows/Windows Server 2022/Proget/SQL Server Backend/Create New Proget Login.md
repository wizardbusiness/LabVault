#### Prerequisites
- ##### [[Check If SQL Server Is Running|SQL Server Service is running]]

#### 1. Decide if you want to use Integrated Authentication or SQL Authentication 
*When to use: Homelab, or if you don't want to deal with integrated windows authentication*

#### *Option A) Integrated Autthentication*

```ad-pscode
collapse: true
	# Commands here	
```

#### *Option B) Use SQL Authentication*

#### 1. Make sure that *integrated authentication* is disabled in proget config

![[Use SQL Authentication Only]]

#### 2. Create a new user 

```ad-pscode
collapse: true
	sqlcmd -S localhost -E -Q "CREATE LOGIN proget_user WITH PASSWORD = 'yourpassword';"
```

#### 3. Edit the connection string in *proget.config* file to include your proget login 

*Note: editing a specific line is tricky with powershell. You could do this with regex but it's easy to screw up especially with xml content. My recommendation is to either use the Powershell XML parser methods (shown below), or just copy-paste into your favorite IDE and edit it there.*


```ad-pscode
	$path = "C:\ProgramData\Inedo\SharedConfig\ProGet.config"
	# store the file content in a variable and cast to xml
	[xml]$config = Get-Content $path
	# use the select node method to select the connection string and target the inner content
	$connectionStr=$config.SelectSingleNode("//ConnectionString").InnerText; 
	# modifies the connection string in place
	$conectionStr += "User Id='youruserid'; Password='yourpassword'";
	# saves the updated xml
	$config.Save($configPath)
	# verify that the connection string was saved correctly
	Get-Content $path
	# should oook something like 
```

```ad-code
title: **xml** `snippet`
***Connection string xml should look something like this***

	<ConnectionString>Data Source=localhost;Database=ProGet;User Id=proget_user;Password='password';</ConnectionString>
```

#### 4. Test the connection - Connect to the proget server via the web browser
###### On a same-domain computer with a web browser go to `http://your-windows-server-ip-address:8624`. If you see the 'Welcome' page for the proget server the user in the connection string was used to successfully authenticate to the server. 
