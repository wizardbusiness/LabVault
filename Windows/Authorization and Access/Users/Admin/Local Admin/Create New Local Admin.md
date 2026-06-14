##### ***Use To***  Text Here

#### Prerequisites
- [[<Internal Link Here>]]
#### Additional Resources
-  [[1] example.com reference alias][1]

[1]: www.example.com
#### See Also
- [[<Related Doc>]]

### *Info*
##### Context for the instructions here

***
### Instructions

#### **Create a new local administrator using powershell**
#### 1. Create a new secure password (at least 8 chars, one symbol, one number)

```ad-pscode
collapse: true
	$password = Read-Host -AsSecureString "Enter PW"
```

#### 2. Create a new local admin user

```ad-pscode
collapse: true
	New-LocalUser "<AdminUsername>" -Password $password -FullName "<Full Admin Name>" -Description "<Optional Description>"
```

#### 3. Add user to Administrators local group
```ad-pscode
collapse: true
	Add-LocalGroupMember -Group "Administrators" -Member "<AdminUsername>"
```
