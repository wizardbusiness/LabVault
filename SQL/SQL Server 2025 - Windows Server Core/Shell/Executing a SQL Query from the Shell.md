##### Prerequisites
- [[Install SQL Server 2025]]
  **-AND-** 
- [[Install sqlcmd]] **-OR-** [[Install SqlServer PSModule]]

*Note: Most documentation examples in this repository use sqlcmd for query execution. It is simply a matter of preference and there is nothing stopping you from executing any query with the SqlServer PS module if you so desire.*
###  **A) Using Powershell**

##### *When to use?* 
- ##### You need to execute a SQL query inside a powershell script
- ##### You need  to work with outputted SQL data as an object, not just as text

```ad-note
###### **Requires [[Install SQL Server Powershell Module|SQL Server Powershell Module]]**
```

```ad-pscode
collapse:true


	#Check if imported
	Get-Module SqlServer
	
	# If not already imported
	Import-Module SqlServer
	
	# Execute a query on the 'AdventureWorks' database
	Invoke-Sqlcmd -ServerInstance localhost -Database AdventureWorks -Query "SELECT TOP 5 * FROM Sales.Customer" | 
	    Where-Object { $_.CustomerID -gt 100 } |
	    Export-Csv -Path "customers.csv"
```
##### Snippet **Breakdown**

- `Import-Module SqlServer` - loads the *PowerShell module* containing *SQL* *cmdlets*
- `Invoke-Sqlcmd` - executes *SQL* and returns *Powershell objects* (not just text)
- `Where-Object { $_.CustomerID -gt 100 }` - filters results using *PowerShell pipeline*
- `Export-Csv` - transforms *object-based output* to structured formats

****
### **B) Using sqlcmd**
##### *When to use?* 
- ##### You need to perform a simple one-off query
- ##### Need result output as text or standard output 
- ##### You need to run SQL scripts or write to ***.sql*** files

```ad-code
title: sqlcmd `snippet`
	sqlcmd -S localhost -d AdventureWorks -Q "SELECT TOP 5 * FROM Sales.Customer"
```
##### Snippet **Breakdown:**

- `sqlcmd` - the **sqlcmd.exe** *executable* which has been added to the *system path*
- `-S localhost` - specifies the  *server instance*
- `-d AdventureWorks` - sets the *database context*
- `-Q "SELECT..."` - executes a *query* and exits immediately



