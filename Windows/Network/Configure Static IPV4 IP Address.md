#### 1. Choose the adapter you want to use (Usually Ethernet or Wlan)
```ad-pscode
	# show all adapters, note the adapter <Name> property
	Get-NetAdapter 
```

#### 2. Check current ip address for the adapter
**Note these property values:**
- **PrefixOrigin**
- **SuffixOrigin**

```ad-pscode
	# Check if IP was assigned manually or handed out by DHCP
	Get-NetIpAddress -InterfaceAlias "<AdapterName>" | Select-Object PrefixOrigin, SuffixOrigin


```

```ad-attention
##### **If prefix and suffix origin for the IP address are 'Manual' that means the IP is already static, you are done.**
```

#### 3. Get the cidr prefix length (default: 24)
```ad-note
*The prefix length is determined by the network. If the computer you're adding the static IP to isn't connected to it yet, you can check on another computer that has already joined the network.* 

**Checking via powershell on connected Windows Machine:** 

`Get-NetAdapter -InterfaceAlias "<AdapterName(Usually 'Wlan'|'Ethernet')>" | Select-Object PrefixLength`


```

#### 4. Get IPV4 default gateway
```ad-pscode
	Get-NetRoute -DestinationPrefix "0.0.0.0/0" | Select-Object NextHop
	# OR  Get-NetIpConfiguration -InterfaceAlias <AdapterName> | Select-Object -ExpandProperty IPV4DefaultGateway | Select-Object NextHop

```

#### 5. If prefix origin and suffix origin are 'DHCP', remove the adapter and net route. 
```ad-pscode
	Remove-NetIpAddress -InterfaceAlias "<AdapterName>" -Confirm:$false
	Remove-NetRoute -InterfaceAlias "<AdapterName>" -Confirm:$false
```

#### 5. Choose static ip address \<network prefix\> + \<unique host ID \>. Check availability 
***Ex: `<network prefix = 172.16.0> + <host id 112> -> 172.16.0.112`***  

```ad-pscode
	# Before testing: clear arp cache to refresh network devices 
    arp -d  
    
	# Test static IP address(s. Test should return False
	Test-NetConnection -ComputerName <unique static ip> -Count 2 -Quiet
```

#### 6. Configure new static IPV4 address for adapter
```ad-pscode
	New-NetIPAddress -InterfaceAlias "<AdapterName>" -Confirm:$false -IpAddress <DomainPrefix + UniqueSuffix ex: 172.16.0.112> `
	-DefaultGateway <DefaultGateway> -PrefixLength 
```

#### 7. Test to confirm that you are connected to the internet 
```ad-pscode
	Test-NetConnection -ComputerName 1.1.1.1 
	# OR 
	ping 1.1.1.1
```

#### 8. Success!





```mehrmaid
graph TD
  A --> B	
```
