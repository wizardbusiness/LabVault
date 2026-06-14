```ad-info
**IIS (Internet Information Services) is Microsofts web server platform.** 
```

##### **1. Install necessary web and .Net features**

```ad-info
***You do not need to do this unless you are using a service that requires it***
```

```ad-pscode
title:`Code Snippet`
collapse:true

	$features = @(
		'Web-Server', 
		'Web-WebServer', 
		'Web-Common-Http', 
		'Web-Default-Doc', 
		'Web-Dir-Browsing', 
		'Web-Http-Errors', 
		'Web-Static-Content', 
		'Web-Health', 
		'Web-Http-Logging', 
		'Web-Performance', 
		'Web-Stat-Compression',
		'Web-Security', 
		'Web-Filtering', 
		'Web-Windows-Auth', 
		'Web-App-Dev',
		'Web-Net-Ext45', 
		'Web-Asp-Net45', 
		'Web-ISAPI-Ext', 
		'Web-ISAPI-Filter', 
		'Web-Mgmt-Tools', 
		'Web-Mgmt-Console'
	)
	
	Install-WindowsFeature $features -IncludeManagementTools
```
