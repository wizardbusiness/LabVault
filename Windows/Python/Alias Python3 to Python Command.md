
### Context
##### Python 3 (released 2008) **broke backwards compatibility** with Python 2. They're fundamentally different. Newer systems default `python` → `python3`, older ones don't. If your system doesn't it's kinda annoying to type python3 if you're only using python3 which will usually be the case for most newer codebases. So this is how you alias it to python.

### Instructions
#### Unix/Linux/Macos Environment

```ad-bash
collapse: true
	alias python='python3'
```

***
#### Windows Powershell

```ad-pscode
collapse: true
	Set-Alias python python3
```



