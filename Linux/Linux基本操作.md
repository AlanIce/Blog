Linux基本操作.md
======

## 永久修改主机名
```shell
[root@localhost ~]# hostnamectl set-hostname <host-name>
```

## 把用户加入sudoers
```shell
[Alan@localhost etc]$ ls -al /etc/sudoers
-r--r----- 1 root root 3530 Apr 20 12:50 /etc/sudoers
```
首先将sudoers的权限改为640
`[root@localhost etc]# chmod u+w sudoers`
然后编辑/etc/sudoers文件，找到
`root	ALL=(ALL)	ALL`
然后在该行下面加一行
`Alan	ALL=(ALL)	ALL`
Alan为要加入sudoers的用户
修改完之后，保存退出，然后再将sudoers的权限该回去，不然的话，Alan第一次使用sudo命令时会提示
```shell
sudo: /etc/sudoers is mode 0640, should be 0440
sudo: no valid sudoers sources found, quitting”
[root@localhost etc]# chmod u-w sudoers
```
之后Alan就可以很好的使用sudo命令了。

## usermod 命令详解
`usermod [options] user_name`
`usermod` 命令修改系统帐户文件来反映通过命令行指定的变化

* 选项(options)

	```shell
	-a|--append  ##把用户追加到某些组中，仅与-G选项一起使用 
	-c|--comment ##修改/etc/passwd文件第五段comment 
	-d|--home    ##修改用户的家目录通常和-m选项一起使用 
	-e|--expiredate  ##指定用户帐号禁用的日期，格式YY-MM-DD 
	-f|--inactive    ##用户密码过期多少天后采用就禁用该帐号，0表示密码已过期就禁用帐号，-1表示禁用此功能，默认值是-1 
	-g|--gid     ##修改用户的gid，改组一定存在
	-G|--groups  ##把用户追加到某些组中，仅与-a选项一起使用 
	-l|--login   ##修改用户的登录名称 
	-L|--lock    ##锁定用户的密码 
	-m|--move-home   ##修改用户的家目录通常和-d选项一起使用 
	-s|--shell   ##修改用户的shell 
	-u|--uid     ##修改用户的uid，该uid必须唯一 
	-U|--unlock  ##解锁用户的密码 
	```

* 示例(Examples):

	1.新建用户test，密码test,另外添加usertest组
	```
	# useradd test 
	# echo "test" | passwd --stdin test 
	# groupadd usertest 
	```
	2.把test用户加入usertest组
	```
	# usermod -aG usertest test ##多个组之间用空格隔开 
	# id test 
	uid=500(test) gid=500(test) groups=500(test),501(usertest)
	```
	3.修改test用户的家目录
	```
	# usermod -md /home/usertest 
	# ls /home 
	usertest
	```
	4.修改用户名
	```
	# usermod -l urchin(新用户名称)  test(原来用户名称) 
	# id urchin 
	uid=500(urchin) gid=500(test) groups=500(test),501(usertest)
	```
	5.锁定urchin的密码
	```
	# sed -n '$p' /etc/shadow 
	urchin:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t \
	Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::: 
	# usermod -L urchin 
	# sed -n '$p' /etc/shadow 
	urchin:!$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t \
	Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::: 
	```
	6.解锁urchin的密码
	```
	# usermod -U urchin 
	# sed -n '$p' /etc/shadow 
	urchin:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t \ 
	Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7:::
	```
	7.修改用户的shell
	```
	# sed '$!d' /etc/passwd 
	urchin:x:500:500::/home/usertest:/bin/bash 
	# usermod -s /bin/sh urchin 
	# sed -n '$p' /etc/passwd 
	urchin:x:500:500::/home/usertest:/bin/sh
	```
	8.修改用户的UID
	```
	# usermod -u 578 urchin (UID必须唯一) 
	# id urchin 
	uid=578(urchin) gid=500(test) groups=500(test),501(usertest)
	``` 
	9,修改用户的GID
	```
	#groupadd -g 578 test1 
	#usermod -g 578 urchin (578组一定要存在) 
	#id urchin 
	uid=578(urchin) gid=578(test1) groups=578(test1),501(usertest)
	```
	10.指定帐号过期日期
	```
	# sed -n '$p' /etc/shadow 
	urchin:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t \ 
	Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::: 
	# usermod -e 2012-09-11 urchin 
	# sed -n '$p' /etc/shadow 
	urchin:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t \ 
	Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::15594:
	```
	11.指定用户帐号密码过期多少天后，禁用该帐号
	```
	# usermod -f 0 urchin 
	# sed -n '$p' /etc/shadow 
	urchin:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t \ 
	Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7:0:15594: 
	```
	**注意(caution)：**
	usermod不允许你改变正在线上的使用者帐号名称。当usermod用来改变userID,必须确认这名user没在电脑上执行任何程序

```
/etc/passwd
user_name:x:uid:gid:commnet:home:shell
/etc/shadow
username:passwd:lastchg:min:max:warn:inactive:expire:flag
--用户名
--密码
--从1970年1月1日起到上次修改密码所经过的天数
--密码再过几天可以被变更(0表示随时可以改变)
--密码再过几天必须被变更(99999表示永不过期)
--密码过期前几天提醒用户(默认为一周)
--密码过期几天后帐号被禁用
--从1970年1月1日算起，多少天后账号失效
```


如何在Centos上安装python3.4
Centos上面默认的Python版本是2.6，本文介绍如何安装3.4版本。

0.下载前准备
需要安装以下库，不然会有问题。

yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
1. 下载Python3.4源码
`wget http://mirrors.sohu.com/python/3.4.1/Python-3.4.1.tar.xz`
2. 解压缩并安装
```
xz -d Python-3.4.1.tar.xz
tar xf Python-3.4.1.tar -C /usr/local/src/
cd /usr/local/src/Python-3.4.1/
./configure --prefix=/usr/local/python34
make -j8 && make install
```
3. 安装的目录
默认情况下，python会安装在

/usr/local/python34
4. 安装PyMySQL
PyMySQL是python的mysql库，安装方法如下：

/usr/local/python34/bin/pip3 install PyMySQL