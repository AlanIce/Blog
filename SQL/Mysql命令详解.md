# 一、 用户权限类
 * 授予权限
 `GRANT privileges ON databasename.tablename TO 'username'@'host';`
    - privileges：用户的操作权限,如SELECT, INSERT ,UPDATE等.如果要授予所的权限则使用ALL
    - databasename：数据库名
    - tablename：表名,如果要授予该用户对所有数据库和表的相应操作权限则可用\*表示, 如\*.\*
    - **注意**：用以上命令授权的用户不能给其它用户授权,如果想让该用户可以授权,用以下命令:
          `GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;`

 * 撤销权限
 `REVOKE privilege ON databasename.tablename FROM 'username'@'host';`

 * 查看授予的权限
 `show grants for root@'localhost';`

 * 删除用户
 `Delete FROM user Where User='test' AND Host='localhost';`

# 二、 表结构变更类
 * 更改表名
 `alter table tablename rename tablenewname;`
 
 * 更改字段类型
 `alter table tablename modify column columnname typename;`

 * 更改字段默认值
 `alter table tablename alter column columnname set default '';`

 * 添加字段
 `alter table tablename add columnname varchar(10) not Null;`

# 三、 数据存取类
 * 从数据库里随机读取几条数据
 `SELECT * FROM table order by rand() limit 20; `

# 四、 数据库编码类
 * 创建UTF-8数据库
 `CREATE DATABASE dbname DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;`
