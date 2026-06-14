#### ***Use When*** 
- ##### An **application** or **service** you are using requires you to create a new [[SQL Glossary#**Database**|database]].
- ##### An **application** or **service**  you are building requires **logical separation** and/or **special permissions** for database entry and retrieval.  
- ##### You are creating a new dataset for a  **business domain** that is decoupled from other domains, ie **'Sales'**
#### Prerequisites
- [[Install SQL Server 2025]]
  **-AND-** 
- [[Install sqlcmd]] **-OR-** [[Install SqlServer PSModule]]

#### Additional Resources:
- [Microsoft Learn - Create A Database](https://learn.microsoft.com/en-us/sql/relational-databases/databases/create-a-database?view=sql-server-ver17)

****
### **Create a new database** 

#### **Additional Context**
- **Windows Server 2022 Core limits your options for query execution to the CLI**
- ***There are multiple ways to execute SQL in the Windows Server Core CLI. See [[Executing a SQL Query from the Shell]] for more information.***
### **A) Create a new simple database**

```ad-code
title: SQL
	
	CREATE DATABASE <DataBaseName>; 
	
```

