从文件获得cookies
===============

```
cookies={}
with open(r'cookies.txt','r') as f:
	for line in f.read().split(';'): 
	#其设置为1就会把字符串拆分成2份 
		name,value = line.strip().split('=', 1) 
		cookies[name] = value
```