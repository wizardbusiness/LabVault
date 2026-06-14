
## Powershell Modules

##### ***Use When** 
- ##### You want a general idea of how powershell modules work
- ##### You want to learn the basics of creating your own modules
#### Prerequisites
- [[Powershell 5.1]] **-OR-** [[Powershell 7+]]]
#### Additional Resources:
- [How to Write a PowerShell Script Module - Microsoft Learn](https://learn.microsoft.com/en-us/powershell/scripting/developer/module/how-to-write-a-powershell-script-module?view=powershell-7.5)

## What is a powershell module? 

##### A module is a way to  package related powershell cmdlets together for portability and reuse. Powershell comes with several base modules preloaded by default, such as Powershell Core. You can install additional modules from repositories like "PSGallery", create your own for internal use, and even publish your modules and share them with other powershell developers.   

##### At its most basic a module is just a valid powershell script saved in a *.psm1* extension. The *.psm1*  extension tells the Powershell engine that it should use module-specific rules and cmdlets for the file. These rules and cmdlets help with things like installing the module, for example on other systems that may have different configurations of powershell and .net on them, or with defining what scope the module is available in. 

*Note: For more complex modules a module manifest file is often used to define module installation and execution rules*
## Example: *Module .psm1 File* 

```ad-pscode
title:**.psm1** `example`
	function Get-PsDriveDiskSpace {
		# Logic here
	} 
	
	Export-ModuleMember -Function Get-PsDriveDiskSpace 
```

## Working with module files 
*The .psm1 file extension automatically gives access to special cmdlets for working with module resources*
- **`Import-Module`**
- **`Export-ModuleMember`**



