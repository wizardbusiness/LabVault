
#### Additional Resources:
- [Microsoft Learn - How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install)
- [Microsoft Learn- WSL Linux Distributions](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions)
-  [Microsoft Learn - WSL Windows Server Installation Guide](https://learn.microsoft.com/en-us/windows/wsl/install-on-server)
#### Prerequisites
- [[Admin Privileges]]
- [[Windows Server 2019 + Desktop Experience ]] **-OR-** [[Windows Server Core 2019 +]]
- [[Windows Powershell 5+]]

****
## Install WSL 

```ad-note
** Manual install is not needed on Windows Server 2025 Desktop Experience.**
**You will also need to install and download a WSL Linux distro, filesize ~1GB**
```

#### 1. Enable WSL

```ad-pscode
collapse: true
	Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux, VirtualMachinePlatform
```

#### 2. Install WSL Kernel Update for WSL 2

```ad-pscode
collapse: true
	Invoke-WebRequest -Uri "https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi" -OutFile ".\wsl_update_x64.msi"
	Start-Process "msiexec.exe" -ArgumentList "/i .\wsl_update_x64.msi /quiet" -NoNewWindow -Wait
```
#### 3. Download a Linux distribution

```ad-note
*VV Use the links in the '**Available distros** listed below to download the distro App Bundle VV*
```

```ad-info
title: **Available Distros**
collapse: true

*Last Updated 12-29-2025 - See [Microsoft Learn- Download Distributions](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions) for most-up-to-date available distros list* 

- Ubuntu:
    - [Ubuntu](https://aka.ms/wslubuntu) (x64, arm64)
    - [Ubuntu 24.04 LTS](https://wslstorestorage.blob.core.windows.net/wslblob/Ubuntu2404-240425.AppxBundle) (x64, arm64)
    - [Ubuntu 22.04 LTS](https://aka.ms/wslubuntu2204) (x64, arm64)
    - [Ubuntu 20.04 LTS](https://aka.ms/wslubuntu2004) (x64, arm64)
    - [Ubuntu 18.04 LTS](https://aka.ms/wsl-ubuntu-1804) (x64)
    - [Ubuntu 18.04 LTS ARM](https://aka.ms/wsl-ubuntu-1804-arm) (arm64)
    - [Ubuntu 16.04](https://aka.ms/wsl-ubuntu-1604) (x64)
- Debian:
    - [Debian GNU/Linux](https://aka.ms/wsl-debian-gnulinux) (x64, arm64)
- Kali Linux:
    - [Kali Linux Rolling](https://aka.ms/wsl-kali-linux-new)
- OracleLinux:
    - [Oracle Linux 9.1](https://publicwsldistros.blob.core.windows.net/wsldistrostorage/OracleLinux_9.1-230428.Appx) (x64)
    - [Oracle Linux 8.7](https://publicwsldistros.blob.core.windows.net/wsldistrostorage/OracleLinux_8.7-230428.Appx) (x64)
    - [Oracle Linux 8.5](https://aka.ms/wsl-oraclelinux-8-5) (x64)
    - [Oracle Linux 7.9](https://aka.ms/wsl-oraclelinux-7-9) (x64)
    - SUSE:
        - openSUSE:
            - [openSUSE Tumbleweed](https://aka.ms/wsl-opensuse-tumbleweed) (x64)
            - [openSUSE Leap 15.6](https://publicwsldistros.blob.core.windows.net/wsldistrostorage/SUSELeap15p6-240801_x64.Appx) (x64)
            - [openSUSE Leap 15.3](https://aka.ms/wsl-opensuseleap15-3) (x64)
            - [openSUSE Leap 15.2](https://aka.ms/wsl-opensuseleap15-2) (x64)
        - SUSE Linux:
            - [SUSE Linux Enterprise Server 15 SP6](https://publicwsldistros.blob.core.windows.net/wsldistrostorage/SUSELinuxEnterprise15SP6-241001_x64.Appx) (x64)
            - [SUSE Linux Enterprise Server 15 SP5](https://publicwsldistros.blob.core.windows.net/wsldistrostorage/SUSELinuxEnterprise15_SP5-240801.Appx) (x64)
            - [SUSE Linux Enterprise Server 15 SP3](https://aka.ms/wsl-SUSELinuxEnterpriseServer15SP3) (x64)
            - [SUSE Linux Enterprise Server 15 SP2](https://aka.ms/wsl-SUSELinuxEnterpriseServer15SP2) (x64)
            - [SUSE Linux Enterprise Server 12](https://aka.ms/wsl-sles-12) (x64)
- Fedora Remix:
    - [Fedora Remix for WSL](https://github.com/WhitewaterFoundry/Fedora-Remix-for-WSL/releases) (x64, arm64)
```

```ad-pscode
collapse: true
	#Change URI and Outfile name to match desired distro 
	Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing
```

#### 4. Extract and install the download Linux distro

```ad-attention
**The `Add-AppPackage` PS Cmdlet will not work on Windows Server Core**. 
```

```ad-pscode
collapse: true
	
```

##### ***Where this guide has been used so far**
- ###### Adding Ansible to Windows Server Core 2022 for home lab
## Changing the WSL Linux distro

