# web.xml加入以下内容
```
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>/WEB-INF/conf/applicationContext.xml</param-value>
</context-param>
```