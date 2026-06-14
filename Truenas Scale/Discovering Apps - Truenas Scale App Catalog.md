
##### ***Use To*** Find and 

#### Prerequisites
- [[<Internal Link Here>]]
#### More Resources
1. [URL Alias][1]

[1]: www.example.com
#### Related Docs
- [[<Related Doc>]]

> [!info]- **More Info - *Why would you want to use a Truenas App?***
>- **Apps can extend the functionality of Truenas Scale, allowing you to run services like PiHole or Restic.**
>- **Apps tagged as *stable* are officially supported, apps tagged as *enterprise* are officially supported and stable enough for busines-critical use, apps tagged *community* are maintained by community members - See example below**
>- **As of version 24.10 'Electric Eel' Truenas Scale apps run in docker containers on Truenas Scale. Prior to that they ran in Kubernetes k3s.Apps in Truenas Core run in FreeBSD jails.** 
>	- Note: *As of March 2026 LLM's will frequently incorrectly state that Truenas Scale apps still run in K3s when giving troubleshooting or configuration advice.*
>
> > [!img]- *Example - App Tags in Catalog*
> > ![[image-10.png|Apps tagged as 'community' and 'stable']] 

****
#### Before you start
- **Log into the Truenas Scale Web UI** 
## Steps
### 1. Go to the Applications catalog. 

####  **> \[Scenario A] Adding your first App -**  Go to the 'Apps' tab and click 'Check Available Apps'
***Note: The button to get to the App catalog is in the middle of the screen if you haven't installed any apps yet.***
~~~ad-img
collapse:true
![[Pasted image 20260203112416.png|Link to app catalog in truenas scale -with no installed apps|893x402]]
~~~
#### **> \[Scenario B] Adding Additional Apps -** Go to the 'Apps' tab and click 'Discover Apps'
~~~ad-img
collapse: true
![[image-9.png|Link to app catalog - with apps already installed|893x402]]
~~~
***
### 2. Search for the app you want to install in the Catalog and click on it to go to its Discover page. 
```ad-img
collapse:true
**Note the multiple search results. In this example searching 'Restic' returns results for a WebUI for Restic and a Restic REST server. Be sure you're installing the service you actually need.**
![[image-12.png|Restic results in truenas web ui app catalog|893x402]]
```

