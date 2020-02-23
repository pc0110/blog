---
title: SSM
date: 2019-10-25 15:24:29
tags: SSM
---

# SSM框架
Spring+SpringMVC+Mybatis
1.Spring+Mbatis:需要整合：将MyBatis的SqlSessionFactory交给Spring

思路：
SqlSessionFactory-> SqlSession->StudentMapper->CRUD
MyBatis最终是通过SqlSessionFactory来操作数据库，就是将MyBatis的SqlSessionFactory交给Spring

#### 1.jar包
Mybatis-spring.jar  spring-tx.jar spring-jdbc.jar  spring-expression.jar
spring-context-support.jar spring-core.jar
spring-context.jar spring-beans.jar spring-aop.jar spring-web.jar commons-logging.jar commons-dbcp.jar  ojdbc.jar mybatis.jar log4j.jar commons-pool.jar 

#### 2.类-表：

#### 3.MyBatis配置文件：

#### 4.通过mapper.xml 将类和表建立起映射关系

#### 5.Spring的配置文件：
通过ContextconfigLocation在web.xml文件中配置Spring的配置文件

# applicationContext.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:jdbc="http://www.springframework.org/schema/jdbc"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc-4.3.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">

</beans>
```
<!-- 数据库连接信息的配置文件 -->

```
   <context:property-placeholder location="classpath:jdbc.properties"/>
```

<!-- 数据库连接池 -->
```
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">

<!-- 指定基本信息 ：jdbc的驱动名、url、数据库名字、密码-->
		<property name="driverClass" value="${driverClass}"></property>
		<property name="jdbcUrl" value="${jdbcUrl}"></property>
		<property name="user" value="${username}"></property>
		<property name="password" value="${password}"></property>

<!--初始化时获取三个连接，取值应在minPoolSize与maxPoolSize之间。Default: 3 -->
<!-- 指定连接池中保留的最大连接数. Default:15-->  
        <property name="maxPoolSize" value="${jdbc.maxPoolSize}"/>  

<!-- 指定连接池中保留的最小连接数-->  
        <property name="minPoolSize" value="${jdbc.minPoolSize}"/> 

<!-- 指定连接池的初始化连接数  取值应在minPoolSize 与 maxPoolSize 之间.Default:3-->  
        <property name="initialPoolSize" value="${jdbc.initialPoolSize}"/>
		
<!-- 当连接池中的连接耗尽的时候c3p0一次同时获取的连接数. Default:3-->  
        <property name="acquireIncrement" value="${jdbc.acquireIncrement}"/>  

 <!-- JDBC的标准,用以控制数据源内加载的PreparedStatements数量。但由于预缓存的statements属于单个
            connection而不是整个连接池所以设置这个参数需要考虑到多方面的因数.如果maxStatements与
            maxStatementsPerConnection均为0,则缓存被关闭。Default:0-->  

        <property name="maxStatements" value="${jdbc.maxStatements}"/>         
<!-- 最大空闲时间,${jdbc.maxIdleTime}(60)秒内未使用则连接被丢弃。若为0则永不丢弃。 Default:0-->  
        <property name="maxIdleTime" value="${jdbc.maxIdleTime}"/> 

<!-- 检查数据库连接池中空闲连接的间隔时间，单位是分，默认值：240，如果要取消则设置为0 --> 
        <property name="idleConnectionTestPeriod" value="${jdbc.idleConnectionTestPeriod}"/> 

<!-- maxStatementsPerConnection定义了连接池内单个连接所拥有的最大缓存statements数。
             Default: 0 -->
		<property name="maxStatementsPerConnection" value="5"></property>  
  
<!-- 数据库连接池过期时间应小于等于mysql的过期时间和mycat的过期时间 -->
        <property name="idleMaxAge" value="20" />

<!-- 每个分区最大的连接数 -->
        <property name="maxConnectionsPerPartition" value="100" />

<!-- 每个分区最小的连接数 -->
        <property name="minConnectionsPerPartition" value="20" />

<!-- 分区数 ，默认值2，最小1，推荐3-4，视应用而定 -->
        <property name="partitionCount" value="1" />

<!-- 每次去拿数据库连接的时候一次性要拿几个,默认值：2 -->
        <property name="acquireIncrement" value="2" />

<!-- 缓存prepared statements的大小，默认值：0 -->
        <property name="statementsCacheSize" value="0" />
        <property name="connectionTimeoutInMs" value="100" />

<!-- 每个分区释放链接助理进程的数量，默认值：3，除非你的一个数据库连接的时间内做了很多工作，
            不然过多的助理进程会影响你的性能 -->
        <property name="releaseHelperThreads" value="3" />
    </bean>
```

<!-- 引入事务的配置 -->
```
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
 
```
<!-- 整合 mybatis -->
```
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 依赖注入数据源，正是上文定义的dataSource -->
        <property name="dataSource" ref="dataSource" />
        <!-- 文件映射器，指定类文件 -->
        <property name="configLocation" value="classpath:/config/sqlmybatis.xml" />
        <!-- 自动扫描mapping.xml文件    指定mybatis的mapper配置文件   -->  
         <property name="mapperLocations" value="classpath:/mapper/**Mapper.xml"></property>
    </bean>
```
<!-- 初始化数据库，是否在服务启动时执行schema.sql -->
```

```
<!--  mybatis自动扫描 将Mapper接口生成代理注入到Spring -->
```
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.cn21.calendar.dao" /><!-- 路径 -->
    </bean>
 
```
<!--配置aop的切面-->
```
 <tx:advice id="txAdvice" transaction-manager="transactionManager">
         <!-- 配置事务属性 -->
         <tx:attributes>
             <!-- 添加事务管理的方法 -->
             <tx:method name="save*" propagation="REQUIRED"/><!-- required 要求 -->
             <tx:method name="delete*" propagation="REQUIRED"/>
             <tx:method name="update*" propagation="REQUIRED"/>
             <tx:method name="select*" read-only="true"/>
         </tx:attributes>
     </tx:advice>
```   

<!-- spring自动扫描：扫描用Service，和Repository注解申明的受管bean -->
Spring配置文件里面写数据源和Mapper.xml文件
之前使用的是mybatis的conf.xml文件->SqlSessionFactory 现在整合 的时候 需要通过Spring管理SqlSessionFactory 因此产生SqlSessionFactory产生的信息放入Spring的配置信息
（替代之前的Mybatis 的配置文件）
在SpringIoc容器中创建Mybatis的核心类SqlSessionFactory

```
<bean id="sqlSessionFactory class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSourse" ref="dataSource">
    </property>
<!--加载mapper路径->
    <property name="maapperLocations" value="org/hhh/mapper/*.xml"></property>
</bean>
```

加载配置文件，配置数据库，在SpringIOC容器中创建Mybatis的核心类SpringSessionFactory:数据源和MyBatis的conf.xml
## 6.使用Spring-Mybatis整合产物开发程序
不在需要conf.xml文件，相应的把数据源通过配置文件db.properties的方式配置到Spring的配置文件中
目标：通过spring产生mybatis最终操作需要的 动态mapper对象（studentMapper对象）
Spring产生 动态mapper对象有三种方法：DAO实现类 继承 SqlSessionDaoSupport 类
SqlSessionDaoSupport类提供了一个属性Sqlsession
## Spring-SpringMVC:就是将SpringMVC各自配置一遍
1.加入mvc所需要的jar
spring-webmvc.jar
2.配置springmvc
web.xml:dispatcherServlet
3.编写springMVC配置文件：
applocationContext-controller.xml:配置视图解析器、基础配置

<!--    配置视图解析器-->
```
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/views/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
```
<!--    配置基础的配置-->
```
    <mvc:annotation-driven></mvc:annotation-driven>
```
