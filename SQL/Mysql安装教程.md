一、 下载软件
------
1. 进入mysql官网，登陆自己的oracle账号，下载Mysql-5.7.14，下载地址：http://dev.mysql.com/downloads/mysql/

2. 将下载好的文件解压到指定目录，笔者解压在C:\software\Mysql\mysql-5.7.14-winx64 

二、 安装过程
------
1. 首先配置环境变量path，将C:\software\Mysql\mysql-5.7.14-winx64\bin配置到自己的path中。

2. 在解压路径下复制my-default.ini,修改名称为my.ini。

3. 打开文件my.ini,添加内容如下：
```
[mysqld]
basedir=C:\\software\Mysql\mysql-5.7.14-winx64
datadir=C:\\software\Mysql\mysql-5.7.14-winx64\data
port=3306
``` 
 - basedir:是上述mysql的解压路径 
 - datadir：后续初始化等数据都会保存在该目录下，在该文件目录下新建data文件夹
 - port：表示连接数据库的端口号

三、 初始化数据库 配置相关信息
----------------
1. 以管理员身份运行windows 命令行

2. 进入mysql的解压缩目录
**提醒**：此处需要进入bin目录，否则后续操作会出现错误。


3. 执行进行初始化，运行命令：`mysqld --initialize --user=mysql --console`
此时会生成root的初始密码，记住此时生成的初始化密码。

4. 安装Mysql服务。运行命令：`mysqld --install MySQL`

5. 此时，可以起动mysql服务，运行命令：`net start mysql`
用户可能会出现如下错误：1. 发生系统错误 2. 系统找不到指定文件。
**错误原因**：如上所述，在运行安装服务命令:`mysqld --install MySQL`时，我们没有进入bin目录，进行安装。
**解决方案**：进入bin目录，首先移除service，运行命令 ：`mysqld --remove` 
重新安装mysql服务，运行命令：`mysqld --install `

四、 登陆数据库
------------

命令行输入   `mysql -u root -p`，
**错误描述**：error 1045 （28000）
**解决方法**：

1. 在my.ini文件中在[mysqld]后一行加入`skip-grant-tables`
此时，关闭mysql服务，再重新启动。  
 
2. 重新登陆， 不需输入密码，直接enter。
 
3. 输入`use mysql`选择mysql数据库：

4. 查询mysql数据库的user表：`select * from user`

5. 此时，我们发现密码字段的名称为authentication_string。有的可能会是password，根据你查询出来的结果为准。

6. 对表user执行update操作： `update user set authentication_string = password("*******") where user="root"  ` 
  
7. 操作成功。退出mysql
 
8. 删除my.ini 文件中的skip-grant-tables ，重新启动mysql服务。

启动成功。至此，mysql在windows中安装成功。

五、 修改密码
-------
进入数据库后输入`use mysql` 后，可能会报错，如下：
**错误描述**：ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
**解决方法**：
 
 1. SET PASSWORD = PASSWORD(‘your new password‘);

 2. ALTER USER ‘root‘@‘localhost‘ PASSWORD EXPIRE NEVER;

 3. flush privileges;

完成以上三步退出再登，使用新设置的密码就行了。

六、 创建用户
--------
 1. 创建用户和数据库
 `CREATE USER 'username'@'host' IDENTIFIED BY 'password';`
 `create database dbname;`
 - username：你将创建的用户名
 - host：指定该用户在哪个主机上可以登陆。
 如果是本地用户可用localhost；
 如果想让该用户可以从任意远程主机登陆,可以使用通配符%。
 - password：该用户的登陆密码。
 密码可以为空，如果为空则该用户可以不需要密码登陆服务器。 
 - dbname：数据库名称

 2. 授权
 `GRANT privileges ON databasename.tablename TO 'username'@'host'` 
 - privileges：用户的操作权限，如SELECT , INSERT , UPDATE 等，如果要授予所的权限则使用ALL。
 - databasename：数据库名
 - tablename：表名
 如果要授予该用户对所有数据库和表的相应操作权限则可用\*表示, 如\*.\*。 
 - **注意**：用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令：
 `GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION; `

 3. 设置与更改用户密码
 `SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');`  
 如果是当前登陆用户用
 `SET PASSWORD = PASSWORD("newpassword");`

 4. 刷新权限表
 `FLUSH PRIVILEGES;`






