---
title: Springboot
date: 2019-11-14 14:38:48
tags: 
- Springboot
- Java web
---


# Springboot入门
@SpringBootApplication开启了Spring的组件扫描和springboot的自动配置功能，相当于将以下三个注解组合在了一起

1、@Configuration：表名该类使用基于Java的配置,将此类作为配置类。

2、@ComponentScan：启用注解扫描。

3、@EnableAutoConfiguration：开启springboot的自动配置功能。

# Springboot的四种属性注入
- @Configuration：声明一个类作为配置类，代替xml文件
- @Autowired  
- @Bean：声明在方法上，将方法的返回值加入Bean容器，代替<bean>标签
- @Value：属性注入
-  @Component 扫描该组件，注册Bean
- @PropertySource：指定外部属性文件。在类上添加@PropertySource("classpath:/jdbc.properties")
  

## @RestController 可以代替 @Controller  和 @Response

# 文件上传

Spring boot 2.0.4 内嵌的Tomcat 8.5 可以直接使用StandardServletMuilpartResolver 而在Springboot提供的文件上传自动化配置类MuilpartAutoCondiguration中也是采用了StandardServletMuilpartResolver 

另一种是CommonsMultpartResolver 

# 自定义error页面

```java
implements ErrorViewResolver
```

# AOP

- Aspect: A modularization of a concern that cuts across multiple classes. Transaction management is a good example of a crosscutting concern in enterprise Java applications. In Spring AOP, aspects are implemented by using regular classes (the [schema-based approach](https://docs.spring.io/spring/docs/5.2.2.RELEASE/spring-framework-reference/core.html#aop-schema)) or regular classes annotated with the `@Aspect` annotation (the [@AspectJ style](https://docs.spring.io/spring/docs/5.2.2.RELEASE/spring-framework-reference/core.html#aop-ataspectj)).
- Join point: A point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.
- Advice: Action taken by an aspect at a particular join point. Different types of advice include “around”, “before” and “after” advice. (Advice types are discussed later.) Many AOP frameworks, including Spring, model an advice as an interceptor and maintain a chain of interceptors around the join point.
- Pointcut: A predicate that matches join points. Advice is associated with a pointcut expression and runs at any join point matched by the pointcut (for example, the execution of a method with a certain name). The concept of join points as matched by pointcut expressions is central to AOP, and Spring uses the AspectJ pointcut expression language by default.
- Introduction: Declaring additional methods or fields on behalf of a type. Spring AOP lets you introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an `IsModified` interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community.)
- Target object: An object being advised by one or more aspects. Also referred to as the “advised object”. Since Spring AOP is implemented by using runtime proxies, this object is always a proxied object.
- AOP proxy: An object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). In the Spring Framework, an AOP proxy is a JDK dynamic proxy or a CGLIB proxy.
- Weaving: linking aspects with other application types or objects to create an advised object. This can be done at compile time (using the AspectJ compiler, for example), load time, or at runtime. Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.

Spring AOP includes the following types of advice:

- Before advice: Advice that runs before a join point but that does not have the ability to prevent execution flow proceeding to the join point (unless it throws an exception).
- After returning advice: Advice to be run after a join point completes normally (for example, if a method returns without throwing an exception).
- After throwing advice: Advice to be executed if a method exits by throwing an exception.
- After (finally) advice: Advice to be executed regardless of the means by which a join point exits (normal or exceptional return).
- Around advice: Advice that surrounds a join point such as a method invocation. This is the most powerful kind of advice. Around advice can perform custom behavior before and after the method invocation. It is also responsible for choosing whether to proceed to the join point or to shortcut the advised method execution by returning its own return value or throwing an exception

# Mapper文件的使用

### 映射文件的顶级元素

- **select：**映射查询语句
- **insert：**映射插入语句
- **update：**映射更新语句
- **delete：**映射删除语句
- **sql：**可以重用的sql代码块
- **resultMap：**最复杂，最有力量的元素，用来描述如何从数据库结果集中加载你的对象
- cache：配置给定命名空间的缓存
- cache-ref：从其他命名空间引用缓存配置

### 命名空间

```xml
<mapper namespace="com.hhh.mapper.BookMapper"></mapper>
```

在Java代码中引用某个sql映射时，使用的亦是含有名称空间的全路径。如：

```xml
session.update("com.hhh.mapper.BookMapper.updateBookById",book)
```



### Select 标签的属性信息

```xml
<select
　　<!-- 
　　　　1. id（必须配置）
　　　　id是命名空间中的唯一标识符，可被用来代表这条语句
　　　　一个命名空间（namespace）对应一个dao接口
　　　　这个id也应该对应dao里面的某个方法（sql相当于方法的实现），因此id应该与方法名一致
　　 -->
　　id="selectUser"

　　<!-- 
　　　　2. parapeterType（可选配置，默认由mybatis自动选择处理）
　　　　将要传入语句的参数的完全限定名或别名，如果不配置，mybatis会通过ParamterHandler根据参数类型默认选择合适的typeHandler进行处理
　　　　paramterType 主要指定参数类型，可以是int, short, long, string等类型，也可以是复杂类型（如对象）
　　 -->
　　parapeterType="int"

　　<!-- 
　　　　3. resultType（resultType 与 resultMap 二选一配置）
　　　　用来指定返回类型，指定的类型可以是基本类型，也可以是java容器，也可以是javabean
　　 -->
　　resultType="hashmap"
　　
　　<!-- 
　　　　4. resultMap（resultType 与 resultMap 二选一配置）
　　　　用于引用我们通过 resultMap 标签定义的映射类型，这也是mybatis组件高级复杂映射的关键
　　 -->
　　resultMap="USER_RESULT_MAP"
　　
　　<!-- 
　　　　5. flushCache（可选配置）
　　　　将其设置为true，任何时候语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false
　　 -->
　　flushCache="false"

　　<!-- 
　　　　6. useCache（可选配置）
　　　　将其设置为true，会导致本条语句的结果被二级缓存，默认值：对select元素为true
　　 -->
　　useCache="true"

　　<!-- 
　　　　7. timeout（可选配置）
　　　　这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数，默认值为：unset（依赖驱动）
　　 -->
　　timeout="10000"

　　<!-- 
　　　　8. fetchSize（可选配置）
　　　　这是尝试影响驱动程序每次批量返回的结果行数和这个设置值相等。默认值为：unset（依赖驱动）
　　 -->
　　fetchSize="256"

　　<!-- 
　　　　9. statementType（可选配置）
　　　　STATEMENT, PREPARED或CALLABLE的一种，这会让MyBatis使用选择Statement, PrearedStatement或CallableStatement，默认值：PREPARED
　　 -->
　　statementType="PREPARED"

　　<!-- 
　　　　10. resultSetType（可选配置）
　　　　FORWARD_ONLY，SCROLL_SENSITIVE 或 SCROLL_INSENSITIVE 中的一个，默认值为：unset（依赖驱动）
　　 -->
　　resultSetType="FORWORD_ONLY"
></select>

```

### resultMap标签的属性信息

```xml
<!-- 
　　1. type 对应的返回类型，可以是javabean, 也可以是其它
　　2. id 必须唯一， 用于标示这个resultMap的唯一性，在使用resultMap的时候，就是通过id引用
　　3. extends 继承其他resultMap标签
 -->
<resultMap type="" id="" extends="">　　
　　<!-- 
　　　　1. id 唯一性，注意啦，这个id用于标示这个javabean对象的唯一性， 不一定会是数据库的主键（不要把它理解为数据库对应表的主键）
　　　　2. property 属性对应javabean的属性名
　　　　3. column 对应数据库表的列名
       （这样，当javabean的属性与数据库对应表的列名不一致的时候，就能通过指定这个保持正常映射了）
　　 -->
　　<id property="" column=""/>
        
　　<!-- 
　　　　result 与id相比，对应普通属性
　　 -->    
　　<result property="" column=""/>
        
　　<!-- 
　　　　constructor 对应javabean中的构造方法
　　 -->
　　<constructor>
　　　　<!-- idArg 对应构造方法中的id参数 -->
       <idArg column=""/>
       <!-- arg 对应构造方法中的普通参数 -->
       <arg column=""/>
   </constructor>
   
   <!-- 
　　　　collection 为关联关系，是实现一对多的关键 
　　　　1. property 为javabean中容器对应字段名
　　　　2. ofType 指定集合中元素的对象类型
　　　　3. select 使用另一个查询封装的结果
　　　　4. column 为数据库中的列名，与select配合使用
    -->
　　<collection property="" column="" ofType="" select="">
　　　　<!-- 
　　　　　　当使用select属性时，无需下面的配置
　　　　 -->
　　　　<id property="" column=""/>
　　　　<result property="" column=""/>
　　</collection>
        
　　<!-- 
　　　　association 为关联关系，是实现一对一的关键
　　　　1. property 为javabean中容器对应字段名
　　　　2. javaType 指定关联的类型，当使用select属性时，无需指定关联的类型
　　　　3. select 使用另一个select查询封装的结果
　　　　4. column 为数据库中的列名，与select配合使用
　　 -->
　　<association property="" column="" javaType="" select="">
　　　　<!-- 
　　　　　　使用select属性时，无需下面的配置
　　　　 -->
　　　　<id property="" column=""/>
　　　　<result property="" column=""/>
　　</association>
</resultMap>
```

### insert标签的属性信息

```xml
<insert
　　<!--
　　　　同 select 标签
　　 -->
　　id="insertProject"

　　<!-- 
　　　　同 select 标签
　　 -->
　　paramterType="projectInfo"
　　
　　<!-- 
　　　　1. useGeneratedKeys（可选配置，与 keyProperty 相配合）
　　　　设置为true，并将 keyProperty 属性设为数据库主键对应的实体对象的属性名称
　　 --> 
　　useGeneratedKeys="true"

　　<!-- 
　　　　2. keyProperty（可选配置，与 useGeneratedKeys 相配合）
　　　　用于获取数据库自动生成的主键
　　 -->
　　keyProperty="projectId"
>
```

### 重用sql标签

```xml
<sql id="userColumns">id,username,password</sql>
```

这个sql片段可以被包含在其他的sql语句中

```xml
<select id="selectProjectList" paramertType="int" resultType="hashmap">
　　SELECT 
    　　<include refid="userColumns"/>
　　FROM 
    　　t_project_002_project_info
</select>
```

### 完全限定名使用别名代替

```xml
<typeAliases>
    <typeAlias type="com.enh.bean.ProjectInfo" alias="projectInfo"/>
</typeAliases>
```

###  



