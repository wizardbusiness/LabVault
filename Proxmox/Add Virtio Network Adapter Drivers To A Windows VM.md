1. Make sure that the virtio media iso is mounted to a drive 

2. Install with pnputil. 
```
pnputil /add-driver <DriveLetter>:\NetKVM\<OS-version>\amd64\*.inf /install
```

#### 1. Make sure that the virtio media iso is mounted to a drive
```ad-pscode
collapse: true
	Get-PsDrive -PsProvider Filesystem
	
	# Should give back any mounted drives, you are looking for .\NetKVM in the root directory
```

#### 2. Once you have the driveletter, install the network drivers with `pnputil`
```ad-pscode
collapse: true
	pnputil /add-driver <DriveLetter>:\NetKVM\<OS-version>\amd64\*.inf /install
```

```ad-tip
**For windows server 2022 use the windows 11 network drivers located in the `'NetKVM\w11'` directory**
```
#### 3. Test that the adapter drivers installed correctly 
```ad-pscode
collapse: true
	Get-NetAdapter
	# Should see output something like in the screenshot below VV
![[image.png|1151x110]]

```

```ad-tip
**Use 'Get-Location' 'Path' property to get absolute path of current directory**
```

```ad-pscode
	$absolutePath = (Get-Location).Path
```

