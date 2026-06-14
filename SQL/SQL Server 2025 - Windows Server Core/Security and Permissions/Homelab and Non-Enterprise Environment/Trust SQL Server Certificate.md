   
```ad-info
##### *Requires **SQL Server** to have already been installed*
```

```ad-pscode
collapse: true
	# Find the cert thumbprint SQL Server is using
	$thumbprint = (Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQLServer\SuperSocketNetLib" -Name Certificate).Certificate
	
	# Export it
	$cert = Get-ChildItem -Path Cert:\LocalMachine\My\$thumbprint
	Export-Certificate -Cert $cert -FilePath "$env:TEMP\sqlserver.cer"
	
	# Import to Trusted Root
	Import-Certificate -FilePath "$env:TEMP\sqlserver.cer" -CertStoreLocation Cert:\LocalMachine\Root
```


 
 



