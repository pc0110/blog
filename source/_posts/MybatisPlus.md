---
title: MybatisPlus
date: 2019-11-01 15:47:40
tags:  MybatisPlus
---



# myabtis+mybatisplus 
增强mybatis但是不做改变
属于ORM框架
1.jar
2.数据表
3.Mybatis的配置文件
4.log4J的日志信息
5.数据库的连接信息
6.Spring配置文件

其中的mybatis-plus 里面包含三个jar ：mybatis  mybatis-spring mybatis-plus

# 切换到Mbatis-Plus 
```
<bean id="sqlSessionFactoryBean" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">

CRUD操作
Mybatis-Plus：只需要写接口不需要写*.xml文件，需要继承继承一个BaseMapper<Student>可以指定一id个泛型
```

MybatisPlus帮你把基本的SqL语句写好了

@TableName("表名")
@TableId(value="字段名"，type=IdTYPE.AUTO)//主键自增，还需要指定数据库里面的自增
@TableField（）指定其他字段 命名规则:stuName->stu_name
注意：
    tuo'luo'feng
    属性stuName-自动转成stu_name需要配置一下
```
<settings>
    <setting namm="mapUnderscoreTocameCase" value="false">
    <setting name="logIml" value="LOG4J">
</settings>
```
或者是把表里面改成stu_name在实体类里面在属性上加入注解@TableField（value="stu_name"）

# MP里面的where语句用Warpper实现
预加载：MP启动时，会预加载常见的CRUD语句，并将这些语句封装到MapperStatement对象中

# AR  activeRecoder形式 :通过实体类来实现CRUD 不需要Mapper对象
需要在实体类中 继承Model<Student>
加载IOC容器

Student student = new Student('qwe',22);
srudent.inster();

MP将主键设置为了Serializable序列化类型，目的：；可以接受常见的类型：8基本类型+String

面向对象的SQL语句：
    wrpper.lanbda().like(Student::getName,"a");
面向SQL的查询语句：
    wrapper.like("stu_name","a");

# MP逆向工程
1.依赖
2.写一个类生成代码

lombok:1.依赖
        2.配置  idea下载一个插件

# 乐观锁:cvs算法  悲观锁：synchorinzed,lock
 悲观锁:把公共需要的值锁住，等待释放后在查询，把并发变成串行 （严重影响效率，不建议使用）
 乐观锁：在每次使用之 进行version检测 没修改一次version会+1

 # SQL 注入器
 通过自己编写的sql语句放入到MP的BaseMapper类中预加载
 自定义方法：
    1.写自己的sql语句+标签名MyDelete extends AbstractMethod
    2.自定义注入器：包含原来的+自定义的
    3.配置 告知MP ，以后使用自定义的注入器

# 逻辑删除
@TableLohic
写一个字段
目的：为了数据安全，假删除只是对数据进行修改操作0->1，真删除是真正的删除操作




