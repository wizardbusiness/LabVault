#### Resources:
- [Install SQL Server 2025 - Microsoft Learn Docs](https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-ver16)
- [Install, configure, or uninstall SQL Server on Windows from the command prompt](https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-ver16)

### **Get The SQL Server 2025 ISO Installation Media**

```ad-important
collapse:none
**Microsoft won't let you download the *SQL SERVER ISO* media directly. Instead you have to download the installer bootstrapper. You *must* use a GUI interface to get the ISO media from the bootstrapper.**

**Once you have it, you'll run it with no arguments to bring up the GUI download interface.**
**`C:\Path\To\Boostrapper\Installer\SQL2025-SSEI-EntDev.exe`**

*Tip: Windows Server Core 2022 supports **GUI installers** *
```

```ad-attention
###### ***!You can skip this section If you already have the sql server media ISO handy!***
```

#### 1. Download the bootstrapper

```ad-pscode
collapse:true
	$baseUrl="https://go.microsoft.com/fwlink/?linkid=2215202&clcid=0x409&culture=en-us&country=us/"
	$installer="SQL2025-SSEI-EntDev.exe"
	
	$installerUrl = $baseUrl + $installer
	
	$downloadDirectory = "C:\Temp" # or wherever
	
	Invoke-WebRequest -Uri $installerUrl -Outfile "$downloadDirectory\$installer"
```

#### 2. Run the bootstrapper

```ad-info
*You must do this via graphical display, **regular ssh won't work**. Easiest way is to log directly into the server/vm. Even headless windows server SKU's support **gui** elements* 
```

```ad-pscode
collapse:true
	# no arguments, we need the GUI installer
	.\path\to\installer\$installer #ex: C:\Temp\SQL2025-SSEI-EntDev.exe
```

#### 3. Download the ISO image via the GUI installer

```ad-img
title:Screenshot
collapse:true
![[SQL-SERVER-Bootstrapper-GUI.png]]
```

****
#### **Install SQL Server From Media ISO**

#### 4. Mount the ISO image as a drive

```ad-pscode
collapse:true
	$imagePath = "C:\Path\To\ISOmedia.iso" # Ex: C:\Temp\SQLServer2025-x64-ENU-EntDev.iso

	Mount-DiskImage -ImagePath $imagePath 
```

****
#### **Install SQL Server Database Engine**

```ad-pscode
collapse:true
	
	$driveLetter = (Get-DiskImage -ImagePath $imagePath | Get-Volume).DriveLetter
	$setupExePath = "$driveLetter\setup.exe # Ex: F:\setup.exe
	& $setupExePath /Q `
	/ACTION=Install /FEATURES=SQLENGINE `
	/INSTANCENAME="MSSQLSERVER" `
	/SQLSVCACCOUNT="NT Service\MSSQLSERVER" `
	/SQLSYSADMINACCOUNTS="badlab\Administrator" `
	/IAcceptSQLServerLicenseTerms
```


### **Optional**

#### **1. Install SqlServer PS Module**
****
![[Install SqlServer PSModule]]

#### **2. Install sqlcmd**


![[Install sqlcmd]]