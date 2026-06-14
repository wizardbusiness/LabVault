## Install Python from CLI using Powershell / Command Prompt in Windows. 
#### Additional Resources:
- [Install Python with CMD or Powershell - Stack Overflow](https://stackoverflow.com/questions/52578270/install-python-with-cmd-or-powershell)

#### 1. Set session web request to use [[Web Term Glossary#TLS - Transport Layer Security|TLS]] [[Web Term Glossary#TLS 1.2|1.2]]
```ad-pscode

	# Force all .NET-based web requests in your PowerShell session to use `TLS 1.2` instead of whatever default the system has configured.
	[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

```

#### 2. Download latest python executable 
```ad-pscode
collapse: true
		# Get latest version of python
	Invoke-WebRequest -Uri "https://www.python.org/ftp/python/3.7.0/python-<latest-version>.exe" -OutFile "c:/temp/python-<latest-version>.exe"
```

#### 3. Install python from executable in quiet mode for current user 
```ad-pscode
collapse: true
	# Install Python silently from downloaded executable
	c:/temp/python-3.7.0.exe /quiet InstallAllUsers=0 InstallLauncherAllUsers=0 PrependPath=1 Include_test=0
```
