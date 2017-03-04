Sublime3_Ubuntu编译选项
======================

## C++
```
{
	"shell_cmd": "g++6 \"${file}\" -o \"${file_path}/${file_base_name}\"",
	"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
	"working_dir": "${file_path}",
	"selector": "source.c, source.c++",
	"variants":
	[
		{
			"name": "Run",
			"shell_cmd": "g++6 \"${file}\" -o \"${file_path}/${file_base_name}\" && gnome-terminal -x bash -c \"${file_path}/${file_base_name};echo;echo 按任意键继续...;read -n 1\""
		}
	]
}
```

## Java
```
{
	"shell_cmd": "javac -encoding UTF-8 -d . \"$file\"",
	"file_regex": "^(...*?):([0-9]*):?([0-9]*)",
	"selector": "source.java",
	"variants": [{
		"name": "Run",
		"shell_cmd": "javac -encoding UTF-8 -d . \"$file\" && gnome-terminal -x bash -c \"java ${file_base_name};echo;echo 按任意键继续...;read -n 1\""
	}]
}
```

## Python
```
{
	"shell_cmd": "gnome-terminal -x bash -c \"python3 $file;echo;echo 按任意键继续...;read -n 1\"",
	"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
	"working_dir": "${file_path}",
	"selector": "source.py",
}
```