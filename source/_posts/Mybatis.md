---
title: Mybatis
date: 2019-10-16 16:09:45
tags: mybatis java
---
# 数据查询语言




//修改为主键
alter table tb_name modify id int auto_increment primary key


```

DQL：数据查询语言 		例如：SELECT语句。（一般不会单独归于一类，因为只有一个语句）。
DML ：数据管理语言		例如：INSERT（插入）、UPDATE（修改）、DELETE（删除）语句。
DCL：数据控制语言		例如：GRANT、REVOKE等语句。
DDL：数据定义语言		例如：CREATE、DROP、ALTER等语句。
TCL：事务控制语言 		例如：COMMIT、ROLLBACK等语句。
```
C: Create
R: Read
U: Update
D：Delete

# 基本数据类型
输入参数：parameter
##### 1.八个基本类型+string
#{任意值}
${value}

##### 2.对象类型
```bash
#{属性名}
${属性名}
#{}自动给String类型加上‘’（自动类型转换）     #{} 
${}原样输出，但是适合于动态排序（动态字段）     '${}' 
Map<String Object> studentmap = new map<>;
```

Procedure  存储过程

jabcType  数据库类型  java类型

# 数据库的 环境切换：
```bash
a.environment配置不同的数据库环境
b.配置Provider别名
c.写不同过的SQL语句
d.在mapper.xml文件中写databaseId="Provider"  （自适应，优先使用带databaseId）

```
# 2.注解方式
推荐使用XML方式
a.将sql语句写在接口的方法上@Select("")
b.将接口的全类名写入mapper,让mybaits知道你的SQL语句是存储在接口中的
<package name = "com.wk.mapper"/> (可以将包中的注解 接口 xml 一次性的引入)

# 3.增删改的返回值问题
a.返回值可以是void  integer boolean long
b.只需要在接口中修改返回值即可

# 4.事务自动提交
a. sqlsessionFactory.opensession()  sqlsession.commit()  //手动提交
b.sqlsessionFactory.opensession(true) //自动提交

# 5.自增问题
```bash
	a.mysql支持自增
	b.oracle同过序列模拟实现 create sequence myseq 
				increment by 1               //步长	
				start with 1; 
	序列自带两个属性：
		nextval:下一个值
		currval:当前值
	insert into student values(myseq.nextval(),................)
	<!--通过oracle的序列化模拟自增-->
	方法一：
   	 <insert id="addStudent" parameterType="com.wk.entity.Student" databaseId="oracle">
      		  <selectKey keyProperty="stuNo" resultType="Integer" order="BEFORE">
           			 select myseq.nextval from dual;
       		 </selectKey>
       		 insert into student(stuNo,stuName,stuAge,graName)
        		values (#{stuNo},#{stuName},#{stuAge},#{graName})
    	</insert>
	方法二：
	    <insert id="addStudent" parameterType="com.wk.entity.Student" databaseId="oracle">
        		<selectKey keyProperty="stuNo" resultType="Integer" order="AFTER">
            			select myseq.currval from dual;
        		</selectKey>
        		insert into student(stuNo,stuName,stuAge,graName)
        		values (myseq.nextval,#{stuName},#{stuAge},#{graName})
   	 </insert>
```

# 6.参数问题：
```bash
将多个参数封装到一个 对象 中 然后使用该对象传递
	a.再mapper.xml中不用写parameterType
	<inster>
		insert into student(stuno,stuName,stuAge,graName)
		values (#{arg0},#{arg1},#{arg2},#{arg2})
	</inster>
	b.可以在 接口中 通过@Param("sNo") 指定SQL中参数的名字
	public abstract Integer addStudent (@Param("sNo") Integer stuNo),
	<inster>
		insert into student(stuno,stuName,stuAge,graName)
		values (#{sNo},.......)
	</inster>
```
# 7.增加null
```bash
oracle:如果是null 则会报 Other	而不是null 

mysql:如果是null则不会报错,没有约束
mybatis中null 是 Other 而 Mysql 可以处理，Oracle 不能处理

a.Oracle这样处理：<inster>
		 insert into student(stuno,stuName)
		 values (#{sNo},#{stuName,jdbcType=NULL})
	           </inster>

//注意标签放置的顺序
b.配置mybatis全局配置文件conf.xml
	<settings>
		<settings name = "jdbcTypeForNull" value = "NULL">
	<settings/>

```
# 8.返回值为Map
```bash
<select id="queryStudentOutByHashMap" parameterType="int" resultType="HashMap">
        		select stuNo "no",stuName "name",stuAge "age" from student
         		where stuNo = #{stuNo}
                    </select> 
	这其中的stuNo是数据库字段名 ，no是stuNo的别名，用于在map 中get 值时使用。map.get("no");
	如果不加别名打印的HashMap 打印的是数据库的字段名

	@MapKey("STUNO") //oracle的元数据（字段名，表名）都是大写
	HashMap<Interger,Student> queryStudentByHashMap();
	
	<select id = "queryStudentByHashMap" resultType ="HashMap" >
		select stnNo,stuName,stuAge from studnet
	</select>

```
# 逆向工程 QBC
逆向工程：
	表，类，接口，mapper.xml文件 当知道一个的时候，其他三个可以自动生成

需要的三个包：
	mybatis-generator-core.jar,mybatis.jar,ojdbc.jar

逆向工程的配置文件：
	generator.xml 

执行：



# XxxMapper代理对象：MapperProxy对象
执行增删改查->M0apperProxy/inkove()-->InvocationHandler:JDK动态代理接口 用到了动态代理模式
动态代理模式：增删改查->代理对象（MapperProxy对象）->代理对象帮我们“代理执行” 增删改查->

# 四个处理器
	StatementHandler ParameterHandler TypeHandler ResultSetHandler

# 自定义插件
	四大核心对象：	StatementHandler ParameterHandler  ResultSetHandler Executor
	
# 通用Mapper
 主键的属性只能是包装类型Integer Long，不能是基本类型
主键策略：有些数据库支持自增，而有些数据库不支持
JDBC支持通过getGenerteKeys方法取回主键的情况
这种情况需要数据库自增，其次是jdbc支持getGenertekeys方法。
常见的如mysql,sqlserver等
用法如下：
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
Oracle中不支持自增用以下方式进行配置：
@Id
@KeySql(sql = "select SEQ_ID.nextval from dual", order = ORDER.BEFORE)
private Integer id;

插入数据时，如果不给主键赋值，是否会回写到对象中

@Transient//该属性不会进行数据库持久化操作，存在于内存中

