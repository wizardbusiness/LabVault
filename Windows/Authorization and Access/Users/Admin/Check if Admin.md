
### **Check if you're logged into an Admin user session***
#### A) CLI (Powershell)

```ad-pscode
collapse:none
  
	$isAdmin = ([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
	$isAdmin # True if admin
```


