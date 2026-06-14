#### 1) After [[Install Tailscale On LInux|installing tailscale ]]run the Tailscale `up` [[Command -- Bash#Definition|command]] in the CLI. 

```ad-bash
~~~bash
sudo tailscale up
~~~
```
#### 2) Authenticate via the link 

1. ***In the terminal you should see some output like this:***
   ```ad-bash
	To authenticate, visit 
		https://login.tailscale.com/a/<random-hex-id>

   ```

2. ***Click the link or paste it into your browser***
#### 3) Login with your proxmox provider account and click 'Connect'
***Note: You can click Device Details to see more about the device you're about to connect***

   ```ad-img
   ![[image-9.png|Connect new device to tailnet]]
   ```

#### 4) If your device connected successfully you'll see a confirmation and then be redirected to your Tailscale Admin center after a few seconds. 

```ad-img
![[image-10.png|Tailscale device successfully added confirmation]]
```

***Note: After the redirect you can now verify machine status in tailscale and see your tailscale device addresses, hostnames etc.***
```ad-img
![[image-11.png|Tailscale device overview]]
```
