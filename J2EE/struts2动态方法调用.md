# 在Struts2中方法调用概括起来主要有三种形式

## 一、指定method属性 
```
<action name="student" class="com.itmyhome.Student" method="add">
    <result name="add">/success.jsp</result>
</action>
```
这样Struts2就会调用Student 中的add方法。

## 二、动态方法调用(DMI) 

用这种方法需要设置一个常量

`<constant name="struts.enable.DynamicMethodInvocation" value="true" />`

动态方法调用是指表单元素的action并不是直接等于某个Action的名字，而是以如下形式来指定Form的action属性
```
<!-- action属性为action!methodName的形式 -->
<action = "action!methodName.action"
```

在struts.xml中定义如下Action
```
<action name="student" class="com.itmyhome.StudentAction">
      <result name="add">/add.jsp</result>
      <result name="delete">/delete.jsp</result>
</action>
```

StudentAction代码为
```
public class StudentAction extends ActionSupport {
    public String add(){
        return "add";
    }
    public String delete(){
        return "delete";
    }
}
```
则在JSP中用如下方式调用方法
```
<a href=”http://student!add.action“>  新增学生</a>
<a href=“http://student!delete.action”> 删除学生</a>
```

## 三、通配符（推荐使用） 
```
<action name="student*" class="com.itmyhome.StudentAction" method="{1}">
            <result name="{1}">/student{1}.jsp</result>
        </action>
```
则在JSP中用如下方式调用方法
```
<a href=“http://studentadd”>  新增学生</a>
<a href=“http://studentdelete”> 删除学生</a>
```
studentadd就会调用StudentAction中的add方法 然后跳转到studentadd.jsp 
studentdelete就会调用StudentAction中的delete方法 然后跳转到studentdelete.jsp 

Struts2支持动态方法调用，它指的是一个Action中有多个方法， 系统根据表单元素给定的action来访问不同的方法，而不用写多个Action。