# Spring对hibernate配置

spring对hibernate配置文件hibernate.cfg.xml的集成相当好，可以在Spring中配置Hibernate的SessionFactory从而取代Hibernate.cfg.xml和HibernateSessionFactory.java

Spring在集成Hibernate时又分为两种形式：（集成后就不需要Hibernate.cfg.xml了）

## 1. 继续使用Hibernate的映射文件*.hbm.xml时扫描映射文件的方法

Spring集成Hibernate时去掉了Hibernate.cfg.xml，此时如果还继续使用Hibernate的映射文件\*.hbm.xml的话，在配置Hibernate的 SessionFactory 时就要配置以何种方式寻找Hibernate映射文件*.hbm.xml

此时spring中配置SessionFactory bean时它对应的class应为org.springframework.orm.hibernate.LocalSessionFactoryBean
例如：

```
<!-- 配置数据源 -->  
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://******:3306/database?useUnicode=true&amp;characterEncoding=utf8"></property>       
    <property name="username" value="root"></property>
    <property name="password" value="123456"></property>
    <property name="maxActive" value="500"></property>
    <property name="maxIdle" value="100"></property>
    <property name="maxWait" value="10000" />
    <property name="removeAbandoned" value="true" />
    <property name="removeAbandonedTimeout" value="60" />
    <property name="logAbandoned" value="true" />
    <property name="validationQuery" value="select 1" />
</bean>

<!-- 定义Hibernate的sessionFactory -->
<bean id="sessionFactory" class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
    <property name="dataSource">
        <ref bean="dataSource" />
    </property>
    <property name="hibernateProperties">
        <props>
            <!-- 数据库连接方言 -->
            <prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
            <!-- 在控制台输出SQL语句 -->
            <prop key="hibernate.show_sql">true</prop>
            <!-- 格式化控制台输出的SQL语句 -->
            <prop key="hibernate.format_sql">true</prop>
            <prop key="hibernate.connection.release_mode">after_statement</prop>
            <prop key="hibernate.hbm2ddl.auto">update</prop>
        </props>
    </property>
    <!--Hibernate映射文件 -->
    <property name="mappingLocations">
        <value>/TBS/Model/*.hbm.xml</value>
    </property>
</bean>

<!-- Action --> 
<bean id="LoginAction" class="PSM.Action.LoginAction">
    <property name="loginDAO">
        <ref local="LoginDAO"/>
    </property>
</bean>
```
 

LocalSessionFactoryBean有好几个属性用来查找hibernate映射文件：mappingResources、mappingLocations、mappingDirectoryLocations与mappingJarLocations 

他们的区别：    

  1. mappingResources：指定classpath下具体映射文件名 
    ```
    <property name="mappingResources">    
        <value>petclinic.hbm.xml </value>    
    </property>    
    ```

  2. mappingLocations：可以指定任何文件路径，并且可以指定前缀：classpath、file等  
    ```
    <property name="mappingLocations">    
        <value>/WEB-INF/petclinic.hbm.xml </value>    
    </property>    
    <property name="mappingLocations">    
        <value>classpath:/com/company/domain/petclinic.hbm.xml </value>    
    </property>
    ```
    也可以用通配符指定，'*'指定一个文件(路径)名，'**'指定多个文件(路径)名，例如：
    ```
    <property name="mappingLocations">    
        <value>classpath:/com/company/domainmaps/*.hbm.xml </value>    
    </property>    
     上面的配置是在com/company/domain包下任何maps路径下的hbm.xml文件都被加载为映射文件 
    ```

  3. mappingDirectoryLocations：指定映射的文件路径   
    ```
    <property name="mappingDirectoryLocations">  
    　<list>    
      　　<value>WEB-INF/HibernateMappings</value>    
      </list>    
    </property>
    ```
     也可以通过classpath来指出    
    ```
    <property name="mappingDirectoryLocations">    
      <list>    
      <value>classpath:/XXX/package/</value>    
      </list>    
    </property>   
    ```

  4. mappingJarLocations：指定加载的映射文件在jar文件中 

 

 

## 2. 使用jpa注解形式的pojo对象，而去掉*.hbm.xml的Hibernate映射文件 时配置的方法 

Spring集成Hibernate时去掉了Hibernate.cfg.xml，此时如果使用jpa注解形式的pojo对象，而去掉Hibernate的映射文件*.hbm.xml的话，在配置Hibernate的SessionFactory时就要配置以何种方式寻找jpa注解形式的pojo映射对象

此时spring中配置SessionFactory bean时它对应的class应为org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean
```
<bean id="sessionFactory" class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
    <property name="dataSource" ref="dataSource" /><!-- 引用数据源 -->
    <property name="packagesToScan">
        <list>
            <value>com.cn.nos.services.pojo*</value><!-- 加载hibernate的jpa注解形式的实体类 -->
        </list>
    </property>
    <property name="hibernateProperties">
        <props>
            <prop key="hibernate.dialect">org.hibernate.dialect.SQLServerDialect</prop>
            <prop key="hibernate.show_sql">true</prop>
            <prop key="hibernate.format_sql">true</prop>
            <!--<prop key="hibernate.current_session_context_class">thread</prop>-->
        </props>
    </property>
</bean>
```
AnnotationSessionFactoryBean中查找jpa注解形式的pojo映射对象的属性有：annotatedClasses、packagesToScan

 1. annotatedClasses：指定classpath下指定的注解映射实体类的类名
    ```
    <property name="annotatedClasses">
         <list>
         　　<value>com.test.ObjectBean</value><!-- 可以在这个list中配置多个 -->
         </list>
    </property>
    ```

 2. packagesToScan指定映射文件的包名
    ```
    <property name="packagesToScan">
        <list>
            <value>com.cn.nos.services.pojo*</value><!-- 加载hibernate的jpa注解形式的实体类 -->
        </list>
    </property>
    ```