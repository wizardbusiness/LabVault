1. Switch to new subnet example 
```ad-code
title: bash `snippet`
	sudo ip addr add 192.168.2.100/24 dev enp11s0
```

2. switch back to previous subnet example 
```ad-code
title: bash `snippet`
	sudo ip addr del 192.168.2.100/24 dev enp11s0
```
