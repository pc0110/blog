---
title: Tomcat-dbcp
date: 2019-10-21 07:43:54
tags: dbcP
---
# 使用Tomcat 配置数据库的数据源

##### 在Tomcat的context.xml文件里配置
```

<Resource name="student"
            auth="Container"
            type="javax.sql.DataSource"
            username="root"
            password="123456"
            driverClassName="org.hsql.jdbcDriver"
            url="jdbc:msyql://127.0.0.1:3306:studnet"
            maxActive="400"
            maxIdle="20"
            maxActiv="5000"
  />
```

##### 在项目的web.xml文件配置
```
<resource-ref>
    <res-ref-name>student</res-ref-name>
    <res-type>javax.sql.DataSource</res-type>
</resource-ref>
```
## 数据源方式
Conrext ctx = new InitialContext()//context.xml
DataSource ds = (DataSource)ctx.lookup("java:comp/env/student")
# 在idea中引入第三的jar包

1.在WEB-INF目录下创建lib文件夹
2.工程结构中-Artifacts-Output Layout-

# dbcp连接池：
1.commons-dbcp-1.4.jar
2.dbcp获取ds 两种方式 BasicDataSource(硬编码)、

BasicDataSourceFactory(配置方式是把所有的信息放到一个Properties文件里)

Properties props = new Properties();//加载配置文件
InputStream input = new DBCPDemo().getClass().getClassLoader().getResourceAsReader("123.properties");
把Properties 文件通过输入流 的形式传入到props.load(input),

# 连接池
Connection connection = DriverManager.getConnection();//链接指向数据库 
用连接池的核心是将链接指向了数据源，而不是数据库
——>DataSource ds = .........
Connection connection = ds.getConnection();
数据库访问核心-> pstmt/stmt->connection->1.直接数据库2.数据源的方式 ds.getConnection()
PreparedStatement pstmt = connection.preparedStatement();





##  总结
1.配置数据源（context.xml）
2.指定数据源（web.xml）
3.用数据库
4.通过数据库获取Connection



