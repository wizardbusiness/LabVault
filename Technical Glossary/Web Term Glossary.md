#### CYBERSECURITY 

##### **` AEAD - Authenticated Encryption with Associated Data `**
  ***Encrypts and authenticates in one operation*** 
****
##### **` BEAST - Browser Exploit Against SSL/TLS `**
  ***Targets CBC mode in TLS 1.0*** 
****
##### **` CCM - Counter with CBC-MAC;`**
***Sequential AEAD cipher mode*** 

****
##### **` CRIME - Compression Ratio Info-leak Made Easy`**
***Exploits TLS compression*** 
****
##### **` GCM - Galois/Counter Mode `**
  ***Fast, parallelizable [[#AEAD - Authenticated Encryption with Associated Data|AEAD]] option. Industry standard for high-performance applications. Encryption and Authentication happen in parallel***
****
##### **` POODLE - Padding Oracle On Downgraded Legacy Encryption `**
  ***Forces downgrade to SSL 3.0*** 
****
##### **` SSL - Secure Sockets Layer `**

  ***Deprecated security protocol that encrypts data transmitted between a web browser and server to protect sensitive information like passwords and credit card numbers from being intercepted. Replaced by [[#TLS - Transport Layer Security|TLS]]*** 

****

##### **` TLS - Transport Layer Security`**
  ***Cryptographic protocol for securing network communications; successor to SSL*** 
****
##### **` TLS 1.0/1.1 `**
- ***Deprecatred in 2018***
- ***Handshake - Vulnerable to BEAST, CRIME, and POODLE attacks***
- ***Ciphers - Allowed weak ciphers like `RC4`, `3DES`, and `MD5`-based signatures*** 
- ***Hash Functions - Used `MD5` and `SHA-1` for signatures (both now broken)***
****
##### **` TLS 1.2 `**
- ***Current most secure version of TLS***
- ***Handshake - Fixed the handshake process to prevent downgrade attacks and padding oracle exploits***
- ***Authentication - Introduces [[#AEAD - Authenticated Encryption with Associated Data|AEAD]] modes like [[#GCM - Galois/Counter Mode|GCM]] and [[#CCM - Counter with CBC-MAC|CCM]]***
- ***Hash Functions - Requires `SHA-256` encryption or better for signatures*** 
****


