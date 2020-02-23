---
title: jdbc
date: 2019-10-16 15:41:36
tags: jdbc java
---
# JDBC:Java Datebase Connectivity
```bash

	Java
	   |
	jdbc 接口，方法，类  api  Connection Statement PrepareStatement  ResultSe
	   |
         Jdbc DriverManager         管理不同 数据库
        |		|	
mysql 驱动程序     oracle驱动程序      各种不同的数据库驱动 会封装在一个JAR包里面 使用的时候直接调用就可以了
mysql数据库         oracle数据库
```
# JDBC  API  的主要功能：
DriverManger:           管理JDBC驱动
Connection:    建立链接
Statement:   增删改查
CallableStatement: 调用数据库中过的存储过程/存储函数  
Result： 结果集

# 具体的驱动类   连接字符串
```bash
oracle.jdbc.OracleDriver				jdbc:oracle:thin:@localhost:1521:ORCL
com.mysql.jdbc.Driver		    		        jdbc:mysql://localhost:3306/数据库实例名	
com.microsoft.sqlserver.jdbc.SQLServerDriver		jdbc:microsoft:sqlserver:localhost:1433;database=数据库的实例名
```
# 版本的坑
private static final String URL = "jdbc:mysql://localhost:3306/test?useUnicode = true&characterEncoding = utf-8&useSSL = false&serverTimezone = GMT";
connection 产生stmt = connection.createStatement();

##### 套路
```bash
try{
a.加载驱动类Class.forName("具体驱动类")；
b.与数据库建立链接connection= DriverManeger.getConnection(...);
c.通过connection,获取操作数据库的对象（Statement\prepareStatement\ callablestatement）
stmt = connection.createStatement();
d.(查询)处理结果集rs=pstmt.executeQuery()
while(re.next()){rs.getXxx(..);}
}catch(ClassNotFoundException e){...}
catch(SQLException e){}
catch(Exception e ){}
finally{
	//打开顺序，与关闭顺序相反
	if(rs!=null)rs.close();
	if(stmt!=null)stmt.close();
	if(connection=null)connection.close();
}
```
##### Statement 操作数据库：
增删改：executeUpdate();
查询：executeQuery();
ResultSet:保存结果集
next():光标下移，判断是否有下一条数据；
previous():true/false
getXXX(字段名|位置):获取具体的字段值

PreparedStatement操作数据库：
public interface PreparedStatement extends Statement

PrepareStatement和Statement 的区别：

Statement后写sql 语句  sql   	executeUpdate(sql)

PrepareStatement 先面写sql语句	（预编译）

PrepareStatement可以用占位符，后面再去赋值

推荐使用PrepareStatement   可以方式SQL 注入


##### 2.CallableStatement:调用 存储过程、存储函数
（connection.prepareCall(存贮过程或者函数的名字)）、
参数格式：
存储过程：（无返回值return 用Out参数替代）
	{call 	存储过程名（参数列表）}
存储函数：（有返回值return）：
	{？=call 存储函数名（函数列表）}
	
```bash
create or replace procedure addTwoNum(num1 in number,num2 in number,result out number)
as
begin
	result:=num1+num2;
end;
```

##### JDBC 调用存储函数的过程：
a.产生调用存储函数的对象 (CallableStatement) cstmt=connection.prepareCall("...");
b.通过setXxx()处理 输出参数值 cstmt.setInt(1,30);
c.通过registerOutParameter(..)处理输出参数的类型
d.cstmt.execute(); 执行
e.接受 输出值（返回值）getXxx（）

##### 调存储函数：

```bash
create or replace function addTwoNum(num1 in number,num2 in number,result out number)
as
begin
	result:=num1+num2;
	return result;
end;
```