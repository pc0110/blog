---
title: filter
date: 2019-10-17 15:41:59
tags: filter
---
# 过滤器
实现一个Filter 接口
重写里面的方法 init destory dofilter
配置过滤器 ，类似Servlet
通过dofilter()处理拦截请求，并通过chain.doFilter(request,response)放行

```
<filter-mapping>
  	<filter-name>UploadServlet</filter-name>
    <url-pattern>UploadServlet</url-pattern> 只拦截Servlet的请求 
    <url-pattern>/*</url-pattern>   拦截一切请求 


    <dispatcher>REQUEST</dispatcher>	拦截HTTP请求 
    <dispatcher>FORWARD</dispatcher>	  拦截通过请求转发方式的请求 
    <dispatcher>INCLUDE</dispatcher>  拦截通过 request.getRequestDispatcher("").include()请求   通过<jsp:include page="..." 
    <dispatcher>ERROR</dispatcher>	 拦截error-page请求 
</filter-mapping>	
```
过滤器中的doFilter方法：ServletRequest
Servlet中的doGet方法：HTTPServletRequest

##### 过滤器链
可以配置多个过滤器，过滤器的顺序是根据<filter-mapping>的先后位置来决定的