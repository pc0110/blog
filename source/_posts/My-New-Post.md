---
title: My Jsp 
date: 2019-10-16 13:51:50 
tags: 
-面试
---
# 架构：
BS：Browser/Server
CS: Client/Server //如果软件升级，那么全部的客户端都要升级 每一台客户端都需要安装软件

# 常见的状态码：
404：资源不存在
403：权限不足
200：一切正常
300：页面重定向
500：内部错误（代码写错了）

# 两种配置项目路径的方式
a.将项目配置到web以外的目录
在conf/server.xml文件中配置
<Context  docBase = "D:\study\JspProject" path = "/JspProject">
//path 相当于webapps(重启)
b.C:\Program Files\Apache SoftwareFoundation\Tomcat8.5\conf\Catalina\localhost 中新建 “项目名.xml”新增一行：
<Context  docBase = "E:\JspProject" path = "/JspProject">

# 虚拟主机
```bash
<Engine name="Catalina" defaultHost="www.test.com">
<Host name="www.test.com"  appBase="E:\JspProject" >
	  <Context docBean="E:\JspProject" parh="/">
</Host>
```
修改本机的Host

# 配置Tomcat
a.将tomcat/lib中的servlet-api.jac加入项目的构建路径
b.右键项目->Build Path ->Add Library ->Server Runtime
# 统一编码字符集

##### 编码分类：
设置Jsp文件的编码（jsp文件中的PageEncoding属性）：jsp->java 
设置浏览器读取jsp文件的编码（jsp文件中的Content属性）
一般将上述设置成一致的编码，推荐使用UTF-8

##### 文本编码：
将整个环境下的编码统一设置
设置某一个项目的编码格式
设置单独的文件编码

jsp元素：脚本Scriplet  注释  指令 

##### 脚本Scriplet 
i:   <%   局部变量java语句 %>
ii：<!%  全局变量，定义方法  %>
iii:  <%=输出表达式%>

一般而言修改 web.xml 配置文件需要重启 tomcat
但是如果修改 Jsp/
注意out.println （）不能回车；
```bash
要想回车：</br>
```
##### 指令
page指令
<%@page.....@>
page 指定的属性
language:jsp页面使用的脚本语言
import:导入类
pageEncoding:jsp文件自身编码 jsp->java
contentType:浏览器解析Jsp的编码

##### 注释
html注释  <!-- 注释 -->
java注释   //   /*
jsp注释   <%-- --%>

# Jsp内置对象
out		输出对象,向客户端输出内容
request		请求对象,存储“客户端向服务端发送的信息”
response 		响应对象
session 		会话对象
appliation 	全局对象
config 		配置对象
page  		当前JSP页面对象（相当于java只中的this）
pageContext	jsp页面容器
exception 	异常对象

# request对象的常见方法：
String getParameter (String name ):根据请求的字段名Key,返回字段值value
String [] getParameterValues(String name):根据请求的字段名KEY，返回多个字段值value（checkbox）
void setCharacterEncoding("编码格式UTF-8"):设置（post）请求编码（tomcat7以前默认的是iso-8859-1,tomvat8以后的是UTF-8）
getRequestDispatcher("b.jsp").forward(request,response);:请求转发的方式跳转页面 A->B
ServletContext getServerContext()：获取项目的ServletContext对象

# get与post的区别：
a.get请求方式会在地址栏里显示请求信息（但是容量有限4-5kb,如果是请求数据存在大文件推荐使用POST）
b.文件上传操作必须是Post 
11.统一请求的编码 request 
##### get
方式请求  如果出现乱码，解决：
a.统一每一个变量的编码：
new String (旧编码，新编码)；
tomcat7 (iso-8859-1)
tomcat8 (utf-8）
b.修改server.xml 一次性的 更改tomcat的编码格式 URIEncoding="UTF-8"
建议使用tomcat 时首先在server.xml文件中修改

##### post
request.setCharacterEncoding("uft-8");
Cookie :name = value
javax.servlet.http.Cookie产生的Cookie
public Cookie(String name, String value)
String name:获取name
String value：获取value
void setMaxAge( int expiry) :设置最大的有效期

# 服务端准备Cookie:
response.addCookie(Cookie cookie)
页面跳转 有两种方式（页面转发，重定向） 
客户端获取cookie:request.getCookies();
a.服务端增加Cookie :response 对象   客户端获取对象：request对象
b.不能直接获取某一个单独对象，只能一次将全部的Cookie 拿到 然后遍历 全部的Cookie  根据名字可以 可以拿到相应的value 

# Session 
session (会话--是服务端产生的用于和客户端之间建立一一对应的关系，) 
session机制：
	客户端在第一次访问服务端的时候，服务端会产生一个Session对象（这其中包含SessionId）,然后服务端还会产生
一个Cookie,当服务端响应客户端的时候就把这个cookie 发送给客户端（包含Jsession=sessionid）
	每个session 都会有唯一的SessionId,
##### Sessionid
sessionid (存在与服务端，想当于客户端cookie里面的Jsessionid,通过sessionid可以区分不同的浏览器) 

cookie(存在于客户端，由有服务端产生，里面可以包含你的用户名和密码，Jsessionid==Sessionid )
建议 cookie 只保存英文和数字 否则需要编码和解码
服务端第一次发现你没有jsessionid 会 new 一个 name=jsessionid 的对象 返回给客户端

##### Session注销
<a href=""invalidate.jsp value="注销">  
session.invalidate();  将整个 session 全部删除  session 失效
session.removeAttribute("name");   //只是删除某个属性值

# cookie 和 session的区别：
标题        cookie		session
保存的位置	 客户端		服务端
安全性		不安全		安全	
保存的内容	String		Object

# application全局对象
四种范围对象（小->大）
pageContext	JSP容器对象（page对象）	
request		请求对象	
response		响应对象
application	全局对象	
以上4个对象共有的方法：
Object getAttribute(String name):根据属性名，或者属性值
void setAttribute(String name, Object obj):设置属性值，（新增，修改）
	setAttribute("a","b")//若之前a对象不存在则new一个a
			若已经存在了则把a的值改为b
void removeAttribute(String name):根据属性名，删除对象

a.
pageContext 当前页面有效(页面跳转之后无效)
b.
request   同一次请求；其他请求无效（请求转发 有效，重定向无效）
c.
session   整个会话期间（切换浏览器/关闭无效）
d.
applivcation  整个项目运行期间都有效（关闭服务器，其他项目）无效
->多个项目共享，重启后仍然有效：JNDI
1.以上的4个范围对象，通过 setAttribute属性赋值，通过getAttribute（）取值
2.尽量使用最小的范围，因为对象的范围越大，造成的性能损耗越大

