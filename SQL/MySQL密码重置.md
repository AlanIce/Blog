MySQL密码重置
=============

## 停止数据库
WIndows下CMD执行`net stop mysql`

## 修改my.cnf
利用文本编辑器打开mysql配置文件my.ini/my.cnf
在mysqld进程配置文件中添加skip-grant-tables。

## 重启数据库
WIndows下CMD执行`net start mysql`

## 修改root密码
重启数据库后可以不用密码直接登陆，执行`mysql -p`可以直接登陆进数据库。

在mysql命令行下执行以下命令修改root密码：
`update mysql.user set password=password('newpassword') where user='root'`

## 重启数据库
密码修改完成后，将my.ini/my.cnf文件中添加的skip-grant-tables语句注释或删除掉，然后重启数据库即可