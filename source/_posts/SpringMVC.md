---
title: SpringMVC
date: 2019-10-16 16:54:04
tags: SpringMVC - java
---

# 1.找相应的JAR包：

新建一个Springmvc 配置文件：Springmvc.xml
选中常用的：beans  aop context mvc

# 2.Servlet
Java类必须符合一定的规范：
a.必须继承 javax.servlet.http.HttpServlet
b.重写其中的doGet()和doPost()方法
Servlet要想使用，必须配置
Servlet2.5:web.xml
Servlet3.0:@WebServlet


### 步骤：
--编写一个类继承Servlet
--重写doPost,doGet方法
--编写web.xml文件中的Servlet-mapping的映射关系
还可以新建一个Servlet，Eclipse会自动生成相关的配置信息

web.xml中的/：		http://localhost:8888/Servlet30   代表项目根路径
jap 中的/：		http://localhost:8888 	     服务器根路径      

构建路径，WebContent:根目录

# Servlet 的生命周期：

加载--初始化-----------        服务          ----------销毁----卸载

加载
初始化  init()， 该方法会在Servlet被实例化后 执行
服务  Service()->doGet()  ,doPost()
销毁   destory(), Servlet被系统回收时执行
卸载

init（）第一次访问 触发只执行一次 可以让tomcat启动时自动执行
--tomcat2.:卸载xml文件里      <load-on-startup>1</load-on-startup>    </servlet>		1先后顺序							
--toncat3：@WebServlet(value = "/WebcomeServlet",loadOnStartup = 1)
Service()->doGet()  ,doPost()  调用几次  执行几次
destory() 关闭tomcat 服务器时会被执行

# 5.Servlet API:  
由两个软件包组成：对应与Http协议的软件包，对应于除了HTTP协议以为ia的其他软件包即Servlet API 可以适用于 任何通信协议
我们学习的Servelt  时位于javax.servlet.http.HttpServlet;

# 6.Servlet继承关系：
ServletConfig():接口
getServletContext getServletContext():获取Servlet上下文对象  application
String getInitParameter(String name) 在当前的Servlet范围内，获取名为name的参数值（初始化参数）

a.ServletContext中常见的方法（application）：
getContextPath(): 相对路径
getRealPath():绝对路径
setAttribute(),getAttribute() String getInitParameter(String name):在当前Servlet范围内，获取名为name的参数值（初始化参数）

Servlet3.0方式 给当前Servlet设置初始值：
@WebServlet(..............,initParams = {@WebInitParam(name="servletparament30",value = "servletpatamentvalues30.....")})
此注解只隶属于Servlet 因此无法为web容器设置初始化参数 要想设置 只能在web.xml文件中配置


# 1.三层架构：
--与MVC设计模式的目标一致：都是为了解耦合，提高代码的复用率
```
表示层（UI）：  
            前台：对应于MVC 的 VIEW 用于和用户交互，界面的显示
			Webcontent

	        后台：对应于MVC 的 Contoller用于控制跳转，调用业务逻辑层
			Servlet ，（Spring MVC 、Struts2）
			xxx.servlet

业务逻辑层(BLL)：对应于MVC的Model  逻辑业务模型
	组装数据访问层	，逻辑性的操作（CRUD：删：查+删）
		xxx.service
        
数据访问层(DAL)：对应于MVC的Model 数据模型
	直接访问数据库的操作，原子性的操作（CRUD）
		xxx.dao
```

##### 三层的关系：高内聚、低耦合
--上层（表示层）：上层依赖下层 持有成员变量，有A 的前提  是必须现有B
--下层（业务逻辑层，数据访问层）

三层架构是典型的架构模式，是典型的上下层关系，存在上依赖下层，但是MVC 不存在上下层的关系，而是相互协作的关系，两者之间没有可比性，是应用于不同领域的技术。

##### 三层优化：
--面向接口开发：先开发接口，再实现类

### 让Spring MVC 访问静态资源，只需要加入两个注解：
<!-- 加载动态资源和静态资源所协调的两个注解-->
```
<mvc:annotation-driven></mvc:annotation-driven>

<mvc:default-servlet-handler></mvc:default-servlet-handler>
```


# Tomcat默认的Servlet
conf/settings.xml


### Spring MVC 自带类型转换器
<S，T> 两个泛型约束 S 代表 的是源数据类型，T 代表的是泛型约束
1.将自定义的类型转换器纳入Spring IOC容器中
2.将容器再放入SpringMVC提供的bean中

@RequestParam("")是一个触发器的桥梁，当你修饰的对象和SpringMVC 发现的对象不一致时他会转换成你需要的对象属性

# 数据格式化的注解
1.配置你所需要的Bean
2.@DateTimeFormat("pattern="yyyy-MM-dd")   @NumberFormat(pattern="###,#")


# 数据校验
实现一个SpringMVC 的一个接口 SpringMVC 会帮你自动搞定
加入这个注解 自动加载一个LocalValidatoryBean 他是ValidatoryFactory的一个实现类
<mvc:annotation-driven></mvc:annotation-driven>

# Spring MVC 文件上传处理方法
```
@RequestMapping(value="testUpload")
public String testUpload(@RequestParam('desc') String desc, @RequestParam('file') String file){
		System.out.println("文件描述信息"+desc);

		InputStream input = file.getInputStream();
		String fileName = file.getOriginalFilename();

		OutStream out = new FileOutStream("d:\\"+fileName);
		
		byte[] bs = new byte[1024];
		int len = -1;

		whiel((len = input.read(bs))!= -1){
			out.write(	bs, 0, len);
		}
}

```
# 拦截器
1.编写拦截器implements HandlerInterceptor
2.配置：将自己写的拦截器 配置到SpringMVC中

拦截器1 拦截请求-拦截器2 拦截请求-请求方法-拦截器2 处理响应-拦截器1处理响应 只会被最后一个拦截器的afterComplrtion渲染

# 异常处理

SpringMVC: HandlerExceptionResolver接口
该接口的每个实现类 都是异常的一种处理方式
ExceptionHandlerExceptionResolver: 提供了@ExceptionHandler注解，并通过该注解处理
//该方法可以捕获ArithmeticException异常

``` 
    @ExceptionHandler({ArithmeticException.class})
    public String handlerArithmeticException(ArithmeticException e){
        System.out.println(e);
    }
```

@ExceptionHandler标识的方法的参数 必须在异常类型（Throwable或其子类）

异常处理路径：最短优先
ExceptionHandler 只能捕获当前类中的异常方法
如果是其他类 则可以加一个注解

b.ResponseStatusExceptionResolver 自定义异常显示界面
@ResponseStatus(value="HttpStatus.FORBIDDEN,reason="数组越界！！") 类 加上这个注解

@ResponseStatus 也可以标识在方法前

















# 总结

如果一个方法用于处理异常， 并且只处理当前类中的异常：@ExceptionHandler
如果一个方法用于处理所有类的异常：类前加@ControllerAdvice 处理异常的方法前加@ExceptionHandler



















