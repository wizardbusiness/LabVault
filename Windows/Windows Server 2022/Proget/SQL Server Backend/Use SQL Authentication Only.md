#### 1. Get the proget *config* file contents

```ad-pscode

	$path = "C:\ProgramData\Inedo\SharedConfig\ProGet.config"
	Get-Content $path
```

```ad-code
title: **xml** `snippet`

*The config file should look similar to this*

	<?xml version="1.0" encoding="utf-8"?>
		<InedoAppConfig>
		  <ConnectionString>
		    Data Source=localhost;Database=Proget;User Id=proget_user; Password='yourpassword';Initial Catalog=ProGet;
		  </ConnectionString>
		  <EncryptionKey>1becfb6696cf7bec1548c769b6595323</EncryptionKey>
		  <WebServer Enabled="true" Urls="http://*:8624/" />
	</InedoAppConfig>


	


```

#### 2. Look for the `<ConnectionString>` element

```ad-code
title:**xml** `snippet`
	<ConnectionString>
		Data Source=localhost;Database=Proget;User Id=proget_user; Password='yourpassword';Initial Catalog=ProGet; Integrated Security=True;
	</ConnectionString>
```

#### 3. If the connection string contains `Integrated Security=True;`, remove that argument

*Note: Powershell isn't great for editing text content beyond basic add or replace. For more advanced text editing use a gui application.* 
```ad-pscode
collapse: true

	$path = "C:\ProgramData\Inedo\SharedConfig\ProGet.config"
	# store the file content in a variable
	$config = Get-Content $path
	# replace the target value with empty string and set the path content to the updated content
	$config -replace 'Integrated Security=True;', '' | Set-Content $path
```

