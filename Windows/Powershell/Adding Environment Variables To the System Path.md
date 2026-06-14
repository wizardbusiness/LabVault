
#### 1. Add a Variable 

##### Option A) Adding a persistent environmental variable to the **\<system\>** path
```ad-pscode
	[Environment]::SetEnvironmentVariable(
	    "PSModulePath",
	    $env:PSModulePath + ";C:\Dev\PSModules",
	    [EnvironmentVariableTarget]::Machine
	)
	
```

##### Option B) Add a temporary environmental variable this session only

```ad-pscode
	# Add a custom dev path (session-only)
	$env:PSModulePath += ";C:\Dev\PSModules"
```


##### Option C) Add a persistent Environmental variable to the **\<user\>** path 

```ad-pscode
	$userenv = [System.Environment]::GetEnvironmentVariable("Path", "User")
	[System.Environment]::SetEnvironmentVariable("PATH", $userenv + ";$env:USERPROFILE\AppData\Local\DebianWSL", "User")
```



#### 2. Exit the session completely and restart it to update the system path. 

#### Troubleshooting

**Fix malformed path: EXAMPLE**
```ad-pscode
	# Fix double semicolons 
	$fixedPath = $currentPath -replace ';;', ';'
```

