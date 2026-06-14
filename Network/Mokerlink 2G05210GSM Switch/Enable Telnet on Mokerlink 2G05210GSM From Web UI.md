### 1. Open your Browser and Access the Mokerlink Web Interface

### 2. Enable Telnet Access

1. **Go to Security -> Management Access -> Management Service**
2. **Check 'Telnet'**
3. **Click Apply to enable the Telnet service 
4. **Then Click 'Save' to save the configuration to device memory**
   ```ad-img
   ![[image-2.png]]
   ```

### 3. Test Telnet Connection

1. **From a device with an interface on the *same subnet* as the Mokerlink 2G05210GSM switch, open a terminal. 

2. **Enter the telnet command** 
   ```ad-bash
	~~~bash
	# your switch's ip here
	telnet 172.16.0.201
	~~~
   ```

3. **You should see the username and password prompt pop up.**
	- *Default Username: **admin***
	- *Default Password: **Admin***

4. **You should see the *Smart Web Managed* Prompt pop up**
   ```ad-bash
   ~~~bash
   Smart WEB Managed #
   ~~~
   ```
   
   5. **You can now enter commands to configure the switch. View Commands ->** [[Mokerlink 2G05210GSM CLI Cheatsheet|Mokerlink CLI Commands Cheatsheet]]
