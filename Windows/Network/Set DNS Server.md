#### 1.  Get the DNS Server IP Address
```ad-pscode
collapse:true
	
	Get-DnsClientServerAddress -InterfaceAlias <interfaceAlias> 
	
```

#### 2. Set the DNS server
*note: Set a fallback dns server like **1.1.1.1** (cloudflare) or **8.8.8.8**  (Google) so you don't lose internet if your primary dns server goes down*
```ad-pscode
collapse:true
	# If not configured
	Set-DnsClientServerAddress -InterfaceAlias <InterfaceAlias> -ServerAddresses ("<DNSServerIpAddress>, ...<additionalFallbacks>)
```


