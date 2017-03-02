Sublime3_Windows编译选项
========================

## C++
```
{
	"cmd": ["g++", "${file}", "-o", "${file_path}/${file_base_name}"],
	"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
	"working_dir": "${file_path}",
	"selector": "source.c, source.c++",
	"shell": true,
	"encoding":"GBK",
	"variants":[{
		"name": "Run",
		"cmd": [ "g++", "${file}", "-o", "${file_path}/${file_base_name}", "&start", "cmd", "/c", "${file_path}/${file_base_name}.exe &echo. &pause"]
	}]
}
```

## Java
```
{
	"cmd": ["javac", "-encoding", "UTF-8", "-d", ".", "$file"],
	"file_regex": "^(...*?):([0-9]*):?([0-9]*)",
	"selector": "source.java",
	"encoding": "GBK",
	"variants": [{
		"name": "Run",
		"cmd": ["javac", "-encoding", "UTF-8", "-d", ".", "$file", "&start", "cmd", "/c", "java ${file_base_name} &echo. &pause"],
		"shell": true,
		"working_dir": "${file_path}"
	}]
}
```

## Python
```
{
	"cmd": ["start", "cmd", "/c", "python $file &echo. &pause"],
	"file_regex": "^(...*?):([0-9]*):?([0-9]*)",
	"selector": "source.py",
	"working_dir": "${file_path}",
	"shell": "true"
}
```