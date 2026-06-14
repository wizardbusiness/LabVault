
1. Install windows server DNS Server Role
2. Promote to DC (If not already)
3. Create a new AD domain (also creates a new primary dns zone w/same name, yay)
4. Add Primary DNS Zone
5. Verify Primary DNS Zone creation
6. (optional) Add DNS forwarders for internet address resolution
7. Set server static ip 
8. Point server dns to own ip (localhost)
9. Test that server resolves addresses correctly
## Examples:
#### A. HomeLab 

##### DNS server and primary DNS Zone setup
```
# Install dns server role with MMC and powershell modules
Install-WindowsFeature -Name DNS -IncludeManagementTools 
# ----------------------------------------------------------------------
# Promote to DC (if not already) with powershell modules
Install-WindowsFeature -Name Ad-Domain-Services -IncludeManagementTools 
Import-Module ADDSDeployment 
# Note that creating a new AD Domain ALSO creates a new DNS domain with the same # name (overloads the domain name), but AD Domains and DNS Domains are NOT the 
# same thing
Install-ADDSForest -DomainName "<youraddomainname>.local" -InstallDns
# Verify DNS zone creation
Get-DnsServerZone
# Should see:
#  - Forward lookup zone -> <youraddomainname>.local
#  - Reverse lookup zone -> <first3octetsofnetworkipinreverseorder>.in-addr.arpa
#  - SRV Record -> _msdcs.<yourdomainname>.local
#  - Default reverse zones -> 
#    - 0.in-addr.arpa
#    - 127.in-addr.arpa 
#    - 255.in-addr.arpa
# ----------------------------------------------------------------------
# Add DNS forwarders for internet address resolution 
Add-DnsServerForwarder -IpAddress 1.1.1.1, 8.8.8.8 # cloudflare, google 
# ----------------------------------------------------------------------
# Set static Ip Address for server, point dns server to itself (localhost)
Get-NetAdapter | Set-DnsClientServerAddress -ServerAddresses 127.0.0.1
# Test that server is resolving dns correctly 
Resolve-DnsName <yourdomainname>.local
Resolve-DnsName google.com
nsolookup <yourdomainname>.local
```
##### Secondary DNS Zone Setup
```
# Add a new dns primary zone
# Use custom top level domain instead of .local to avoid conflict with Bonjour and mDNS.Ex: services.internal
Add-DnsServerPrimaryZone -Name <secondarydomainanme>.<customdomain> -ReplicationScope Forest

# Add 'A' Record for device that resolves through new secondary zone
Add-DnsServerPrimaryRecordA -Name "<servicename>" -ZoneName <secondarydomainanme>.<customdomain> -IPV4Address "<targethostipaddress"
```

