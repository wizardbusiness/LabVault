
1. Log in to truenas scale Web UI as an admin with access to the app store
2. Go to the 'Apps' tab in the Web UI
3. Search for Syncthing
4. Click on 'Syncthing' in the results
5. On the 'Syncthing' App page click 'Install'. *Note: if you get an error like \[ENOENT] Unable to locate 'syncthing', you probably need to [[Refreshing the Catalog - Truenas Scale App Catalog|refresh the app]] catalog.*
6. Choose app name and version
	![[image-3.png]]
7. Choose Timezone. Leave environmental variables blank
	![[image-4.png]]
8. Change User ID and Group ID to match the owner of the dataset that synchting will be accessing *within the truenas scale pool*. So for example, if you are syncing data from the dataset 'My Share' and the owner is 'WizBiz', you need to add the GUID for 'WizBiz'
	![[image-5.png]]
9. Set Web UI 'Port Bind Mode' to 'Publish port on the host for external access', and port number to syncthing default port '8384' (Or keep default port).
	![[image-6.png]]
10.  Leave 'Host IPs' blank - will default to truenas scale host ip. Only need to choose specific IP's if you have multiple Truenas Scale servers running together and want to limit which hosts allow syncthing access for security purposes.
	![[image-8.png]]
11. Check 'Host Port' underneath the TCP and UDP Port sections - Syncthing will listen directly on the host's ports, instead of the host forwarding its ports to ports inside the container. Less isolated, but also simpler. Fine for homelab.
	![[image-9.png]]
12. Leave DNS Options Empty - will default to host dns. 
	![[image-10.png]]
13. Choose 'No Certificate' - This is for HTTPS, if syncing across public networks. Syncthing uses its own TLS certicate system internally instead.
	![[image-11.png]]
 14. Keep 'Syncthing Config Storage' set to 'ixVolume (Dataset created automatically by the system)' - This is just where  Syncthing will store its config data files, it is NOT the folder that you want to sync
	 ![[image-14.png]]

15. Keep 'Enable ACL' unchecked. You're just going to use the underlying Unix permissions you configured with User ID and Group ID
	![[image-15.png]]
16. Additional Storage. This is for defining what data syncthing actually has access to within the truenas storage pool. Click 'Add'
	![[image-16.png]]
17. Under 'Type' choose 'Host Path' 
	![[image-17.png]]
18. Leave 'Read-Only' unchecked unless you want syncthing to only be able to read, not write data. 
	![[image-18.png]]
19. Under Mount point choose an arbitrary name - i've chosen '/sync' This is simply the name of the mount point that the container will use to access your dataset data on the Truenas Host for syncing. 
	![[image-19.png]]
20. Leave 'Enable ACL' unchecked
	![[image-20.png]]
21. Choose 'Mount Path'. This is the actual directory which syncthing will have access to the contents of. Syncthing will not be able to access files or directories outside this folder. *Note: If you want, you can add more than one 'Additional Storage' location. For this purpose i've only chosen one.*
22. Skip Labels configuration unless you're using this app with Kubernetes or a container orchestration system
23. Leave CPU's at 2 Cores - Defines the MOST CPU cores that syncthing can use. More than enough, syncthing is mostly I/O bound
	![[image-21.png]]
24. Leave RAM at 4GB - Defines the MOST RAM that syncthing can use. Adequate unless you are connecting to a very large number of devices or have a very large synced directory - > 1 million files, or >40 devices syncing (add +2 GB per 20 devices)
	![[image-22.png]]
25. Click 'Install' - May take up to several minutes
	![[image-23.png]]
26. Success! You should now see the syncthing App listed and running under Installed Applications. Click the 'Web UI' button to go the syncthing WebUI
	![[image-24.png]]

#### More Resources
1. [Claude Sonnet 4.5][1]

[1]: https://claude.ai/share/a1a6f1a8-b87d-4ec8-9982-9399ad5d23b2

