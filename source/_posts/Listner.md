---
title: Listener
date: 2019-10-18 08:17:12
tags: Jsp 
---
# 监听器
范围对象有四种:pageContext request session application
实现三个接口
编写xml
##### 监听对象:request session application
request:ServletRequestListener
session:HttpSessionListener
application:ServletContextListener

###### 监听对象属性的变更：
request:ServletRequesAttributetListener
session:HttpSessionAttributeListener
application:ServletContextAttributeListener

session的四中状态：
监听session 的绑定和解绑：HttpSessionBingdingListener        不需要配置xml
a.session.setAttribute("a",);  将a对象  绑定  到session 中
b.session.removeAttribute("a");  将a对象 从 session 中解绑
监听session对象的钝化、活化，HttpSessionActivationListener  不需要配置xml
c.钝化
d.活化
如何钝化、活化 ： 配置 tomcat安装目录context.xml
```
<Manager calssName="org.apache.catalina.session.PersistentManager" maxIdleSwao="5">   最大空闲时间为五秒  超过该时间将会被钝化
    <Store  className="org.apache.catlina.session.FileStore" directory="lq"/>                 通过该类具体实现钝化操作         相对路径(绝对路径)：C:\Program Files\Apache Software Foundation\Tomcat 8.5\work\Catalina\localhost\UpAndDown
</Manager>
```

### 总结:
    钝化、活化的本质就是序列化、反序列化 需要实现Serializable接口
    钝化、活化 就是通过序列化 把session 读取到硬盘，然后用过反序列化 从硬盘中读取出来
    HttpSessionActivationListener只是负责在session在钝化、活化时被监听 
