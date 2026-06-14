
#### A) Removing a driver

```ad-pscode

	# Get the network adapter's instance ID
	$adapter = Get-NetAdapter -Name "Ethernet"
	$instanceId = (Get-PnpDevice | Where-Object {$_.FriendlyName -eq $adapter.InterfaceDescription}).InstanceId
	
	# Display it to verify
	$instanceId
```

