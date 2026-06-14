URL: https://raw.githubusercontent.com/microsoft/Windows-Containers/Main/helpful_tools/Install-DockerCE/install-docker-ce.ps1


##### **1. Download docker CE**
```ad-hint
title:`Code Snippet`
collapse:true
	$url = 'https://raw.githubusercontent.com/microsoft/Windows-Containers/Main/helpful_tools/Install-DockerCE/install-docker-ce.ps1'
	Invoke-WebRequest -uri $url <download\directory>\Install-Docker-CE.ps1	
```

##### Run the docker CE install script
```ad-tip
title:`Code Snippet`
collapse:true
	. <yourdownloaddirectory>/Install-Docker-CE.ps1
```
