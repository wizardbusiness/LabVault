##### ***Use To***  Create a new PSCredential within a Powershell session

#### Prerequisites
- [[Check if Admin|Administrator Privileges]]
#### Additional Resources
-  [[1] example.com reference alias][1]

[1]: www.example.com
#### See Also
- [[<Related Doc>]]

### *Info*
##### Credentials are used to authenticate actions 

***
### Instructions

#### A) Using Cmdlet - `Get-Credential`
##### 1. Invoke `Get-Credential` and enter a username and password in the form GUI. Store result in a variable
***Note: Requires [[Input > User#Definition|user input]] and GUI access. Will not work in headless environments or scripts***
```ad-pscode
collapse: true
~~~powershell
# resuable credential
$credential = Get-Credential
~~~
```

***
#### B) Using .Net Class - `PSCredential` 

##### 1. Store password as a secure string
```ad-pscode
collapse: true
~~~powershell
$password = ConvertTo-SecureString "<password>" -AsPlaintext -Force
~~~
```
```ad-warning
**Don't use `ConvertTo-SecureString` in production, it requires the password to be exposed as plaintext at some point in the session creating a security vulnerability. Use a key vault like Azure Key Vault, or the Credential-Manager powershell module**
```
##### 2. Create the credential and store in a variable 
```ad-pscode
collapse: true
~~~powershell
$credential = [PSCredential]::New("<Username>", $password)
~~~
```

