##### ***Use To***  Add a computer to the domain Active Directory on a Domain Controller

#### Prerequisites
- [[Powershell 5.1+]]
- [[Check if Admin|Administrator Privileges]]
- [[Domain Name]]
- [[Domain Controller Admin Username and Password]]
- [[Domain Controller IPV4 Address]]
#### Additional Resources
-  [[1] example.com reference alias][1]

[1]: www.example.com
#### See Also
- [[<Related Doc>]]

### *Info*
##### Context for the instructions here

***
### Instructions

#### **Join the computer to the domain**

#### 1. On the computer you want to join to the domain, create a new credential containing the administrator username and password for the Domain Controller. 
![[Create New Credential#A) Using `Get-Credential`]]
#### **-OR-**
![[Create New Credential#B)Using an instance of the `PSCredential` .Net class]]

#### 2. Add the computer to the domain using the `Add-Computer` cmdlet and restart
```ad-pscode
collapse: true
	Add-Computer -Domain <yourdomainname> -Credential $credential -Restart
```

#### 3. On the Domain Controller - Verify that the computer was added to the domain successfully 
```ad-pscode
collapse: true
	Get-AdComputer -Filter "Name -like '*<computername>*'"
```

