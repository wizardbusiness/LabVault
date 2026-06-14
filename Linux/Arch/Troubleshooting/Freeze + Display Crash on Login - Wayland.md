#### **Error:** Freeze on login after fresh boot - couldn't login. Progressed to full display-driver crasher (No Signal).  

##### What it meant: **Wayland was crashing**
##### What I was doing when I encountered it
- **Logging into Wayland desktop - only occurred on fresh reboot.**
##### Why it occurred
- **GNOME shell running on Nvidia 4070TI Super Discrete GPU under Wayland.**
- **NVIDIA on Linux has stability issues especially when running as the primary display device under Wayland.** 
##### Second-order effect symptoms
*Note: Initially thought these were the cause, but they were just symptoms*
- **Windscribe VPN helper service crashing on startup.**
- 

##### Fix Process
###### Checking crash logs
1. Switching to terminal on login screen via `ctrl` + `shift` + `2`. 
2. Running `journalctl`
- **Started by looking at crash logs in journalctl**
- **Started `WSL` session and retried command**
##### More Info
- [Source][1]
- [Source][2]

[1]: www.source.com
[2]: www.source.com
##### **Error keywords to look for**
- `Frozen runpy` 
- `check_block_io()` 
- `OSError`
- `[WinError 1] incorrect function`
##### ***Error Screenshot***

```ad-error 
![[image.png]]
```





