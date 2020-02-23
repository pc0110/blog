---
title: JSTL
date: 2019-10-17 14:39:20
tags: JSTL 
---
# JSTL 
比EL更加强大 需要两个jar包：jstl.jar、standard.jar
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```
其中的prefix="c"：前缀
核心标签库：    通用标签库      条件标签库       迭代标签库
a.通用标签库  在某个作用域之中，给某个变量赋值
```
	<%
		request.setAttribute("name", "asdas");
	%>
	<c:set var = "name" value="zhangsan" scope="requset"></c:set>
	${requestScope.name}
```
在某个作用域之中（4个范围对象），给某个对象赋值(这种方法不能指定scope属性)
```
<c:set target="${requestScope.name}" property="sname" values="zxs"></c:set>

传统EL:${requestScope.student}
c:out显示：<c:out value="${requestScope.student}"/>
c:out显示不存在的数据 <c:out value=${requestScope.student} defalut = "zs">
<a href = "https://www.baidu.com">百度</a>
<c:out value='<a href = "https://www.baidu.com">百度</a>' escapeXml = "true"/>
<c:remove vat="a" scope="request">
```
b.条件标签库
```
<c:choose>
    <c:when test="${requestScope.role=='老师'}">
        老师代码。。。。。
    </c:when>
     <c:when test="${requestScope.role eq '学生'}">
        学生代码。。。。。
    </c:when>
    <c:otherwise>
        管理员等其他人员。。。
    </c:otherswise>

</c:choose>
```
在使用test="" 注意后面不要有空格
c.迭代标签库
```
<c:forEach begin="0" end="5" step="1">
    test...
</c:forEach>
<c:forEach begin="0" end="5" varStatus="status">
    test...
</c:forEach>

<c:forEach var="student" items="${requestScope.students}">
    ${student.sname}--${student.sno}
</c:forEach>
```
items=""后面不要有空格