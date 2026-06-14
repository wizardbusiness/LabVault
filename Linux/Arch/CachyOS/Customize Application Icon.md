### ***Add a custom icon for an application***

### *Easy Way - Edit app .desktop file*

```ad-warning
***Will break if you switch themes***
```

#### 1. Copy the system desktop file for target app to user directory to override its system desktop file

> [!bash]- bash
> ~~~bash
> cp /usr/share/applications/<your-application>.desktop ~/.local/share/applications/
> ~~~
#### 2. Edit the file in a text editor like Nano
```ad-bash
collapse: true
	nano ~/.local/share/applications/<your-application>.desktop
```

#### 3. Edit the \<Icon\> path to point to the new file
```ad-bash
collapse: true
	
	# Example <your-application>.desktop contents
	
	[Desktop Entry]
	Name=My Application
	Exec=/usr/bin/myapp
	Icon=/path/to/custom/icon.png
	Type=Application
	Categories=Utility;
```

### *Elegant Way - Add icon to app icon themes*

```ad-info
***Will keep working if you switch themes***
```

#### 1. Create a new icon directory
```ad-bash
collapse: true
	# Create
	mkdir -p ~/.local/share/icons/hicolor/512/apps/	
```

#### 2. Copy your icon to the icon directory
```ad-bash
collapse: true
	cp myicon.png ~/.local/share/icons/hicolor/256x256/apps/<your-application>.png	
```

#### 3. Update the icon GTK Cache
```ad-bash
collapse: true
	gtk-update-icon-cache ~/.local/share/icons/hicolor/
```

#### 4. In the system .desktop file for target app use \<your-application\> as defined in the icon directory from step 1
**Open the system /<your-application/>.desktop file**
```ad-bash
collapse: true
nano /usr/share/applications/\<your-application\>.desktop 
```


```ad-bash
collapse: true
	
	# Example <your-application>.desktop contents
	
	[Desktop Entry]
	Name=My Application
	Exec=/usr/bin/myapp
	Icon=myapp
	Type=Application
	Categories=Utility;
```











