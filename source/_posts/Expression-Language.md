---
title: Expression Language
date: 2019-10-17 09:51:16
tags: el 
---

#  EL 表达式 --Expression Language  
可以代替jsp页面中的java代码
Servlet（增加数据）-> jsp（显示数据）
EL 示例：
```
$(resquestScope.student.address.schoolAddress)<br/>
$(域对象.域对象的属性名.属性.属性.级联属性))
```
### EL操作符：
点操作符 : 使用方便
[]操作符 : 功能强大，可以使用特殊符号，可以获取变量值
示例：
```
    点操作符:${studnetSpoce.my-name}
    []操作符:${studentScope['my-name']}，${studentScope[name]}（name是变量），可以获取数组元素
```
### 获取map属性
```
Map<String,Object> map = new HashMap<>();
map.put("cn","中国")；
```
### 关系运算符 逻辑运算符
Empty运算符：判断一个值是否为NULL、不存在-> true
### EL 表达式的隐式对象
```
a.作用域访问对象（EL域对象）:pageScope      requestScope       sessionScope       appicatioScope
    如果不指定域对象：则会默认根据从小到大的顺序依次取值
b.参数访问对象：获取表单数据(resquest.getParament()  request.getParamentValues() )
                            ${param}                 ${paramValues}
c.Jsp隐式对象：pageContext
    在jsp中通过pageContext获取其他的jsp隐式对象，因此如果要通过EL 获取jsp对象可以通过pageContext获取，
    例如：   ${pageContext.getSession()}->${pageContext.session}
            ${pageContext.getResponse} ->${pageContext.response}
            可以使用此方法-级联获取方法：
            ${pageContext.request.serverport}



    
```