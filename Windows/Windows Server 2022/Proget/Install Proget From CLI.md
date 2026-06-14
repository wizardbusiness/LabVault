
#### Prerequisites
- [[Install SQL Server 2025]]]
- [External Link Here]()


## Configure SQL Server

#### Resources
- [Proget Hub Installer Zip Archive DownloadLink]("https://proget.inedo.com/upack/Products/download/InedoReleases/DesktopHub?contentOnly=zip&latest")
- [Proget Docs](https://docs.inedo.com/) 
- [Proget Silent Installation Options](https://docs.inedo.com/docs/installation/windows/inedo-hub/silent)

#### 1. (If not already done) Install the Sql Server *Powershell Module* for easier SQL execution 

```ad-pscode
collapse: true
	Install-Module -Name SqlServer -Force # [Y] to install nuget if asked
```

#### 2. Create a new SQL *database* for Proget

```ad-pscode
collapse: true
	Invoke-Sqlcmd -ServerInstance "LocalHost" -Query "CREATE DATABASE ProGet;"
```
#### 3. Download Inedo Hub installer 

```ad-pscode
collapse: true
	# create working directory
	mkdir C:\InedoHub
	cd C:\InedoHub
	
	# download and extract file to working directory
	Invoke-WebRequest "https://proget.inedo.com/upack/Products/download/InedoReleases/DesktopHub?contentOnly=zip&latest" -OutFile C:\InedoHub\InedoHub.zip
```

#### 4. Extract Inedo Hub content

```ad-pscode
collapse: true
	Expand-Archive -Path InedoHub.zip -DestinationPath C:\InedoHub
```

#### 5. Install Proget (V6 or greater Recommended)

```ad-pscode
collapse: true
	
	# perform silent installation
	hub.exe install ProGet:5.2.3 --ConnectionString="Data Source=localhost; Integrated Security=True;"

```

### **Troubleshooting**

- ##### **Bad Credentials**
```ad-error
title: Authorization Issues
	
Error Msg: `Invoke-Sqlcmd : Login failed for user 'BADLAB-PSREPO-1\Administrator'.`
***Explanation:*** **The credentials you entered for `/SQLSYSADMINACCOUNTS are bad.**  
***Fix:*** *Add the correct credentials / group manually*

***Additional Thoughts:*** *If proget is installed on a device that is part of a **managed domain**, make sure the credentials are valid admin credentials on the current device.*

```
