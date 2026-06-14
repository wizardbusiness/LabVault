##### ***Use To*** Add an arch linux device to your peer-to-peer mesh VPN running on top of WireGaurd 

	#### More Resources
	#### Prerequisites
- [[Create a Tailscale Login]]
1. [Install Tailscale on Arch Linux][1]
2
.  [What Problem Does Tailscale Solve - Claude Sonnet 4.6][1]

[1]: https://tailscale.com/docs/install/arch
[2]: https://claude.ai/share/576e84ef-8396-410b-adb6-ff8f4c48b747

#### Related Docs
- [[<Related Doc>]]
### *Info*
##### This guide is based on the one from the [Tailscale official documentation][1]. It adds some QOL features such as code snippet explanations. 

****
### 1. Install Tailscale client
```ad-bash
~~~bash
sudo pacman -S tailscale
~~~
```

### 2. Configure Tailscale service to start automatically when Arch Linux boots
```ad-bash
~~~bash
sudo systemctl enable --now tailscaled
~~~
```

### 3. Connect your device to your Tailscale network
#### 1. Start the tailscale client
***Note: You'll be prompted to authenticate in your browser as soon as you do this***
```ad-bash
~~~bash
sudo tailscale up
~~~
```
#### 2. Authenticate using the same login as you used to [[Create a Tailscale Login]]

#### 3. (*New Tailscale network Only*) If this is your first device running Tailscale, you'll be prompted to add a second device in the browser. 
**Choose an option and folow the instructions for the next device you want to add.**
```ad-img
![[image-6.png]]
```

