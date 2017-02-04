pymysql模块示例
======

```
import pymysql

try:
	conn= pymysql.connect(host='localhost', port=3306, user='DBSAdmin', passwd='admin', charset='UTF8', db='dbs')
	cur=conn.cursor()                              #获取一个游标对象
	cur.execute("INSERT INTO nameage VALUES('小明', 15),('小洪', 17),('小高', 16),('小刚', 15)")#插入数据

	cur.execute("SELECT * FROM nameage")
	data=cur.fetchall()

	for row in data:
		print('%s\t%s' %row)
except Exception as e:
	print("发生异常")
finally:
	cur.close()                                    #关闭游标
	conn.commit()                                  #向数据库中提交任何未解决的事务，对不支持事务的数据库不进行任何操作
	conn.close()                                   #关闭到数据库的连接，释放数据库资源
```