# 修改struts.xml位置

web.xml 中改动如下即可：
```
<filter>
    <filter-name>struts2</filter-name>
    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    <init-param> 
        <param-name>config</param-name> 
        <param-value>struts-default.xml,struts-plugin.xml,../conf/struts.xml</param-value> 
    </init-param> 
</filter>
```