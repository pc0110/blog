---
title: paging
date: 2019-10-16 16:21:28
tags: page java
---

# 1.分页
分析 要实现分页要知道数据是在哪里 开始和结束

假设每条显示10条数据


### mysql  和 ORACLE的 SQL 语句不太一样 
```

	msyql 从0开始计数  其中有Limit关键字（Oracle没有）

		select * from student limit  页数*10，10；
	
	Oracle 是从1开始的 
		select * from student where sno>=(n-1)*10+1 and sno<=n*10;(此种写法是有漏洞的，ID值必须是连续的，假如中间有学生退学之类的情况，中间就会出现空位，存在BUG)
		select rownum,t.* from student t where sno<=(n-1)*10+1 and sno>=n*10;这种写法会根绝rownum列进行排序，但是伪列的顺序会被打乱、rownum只能查询小于的数据不能查询大于的数据
		解决方案：分开使用，先只排序，
		a.select s.* from student s order by sno asc ;
		b.select  rownum,t.*from (select s.* from student s order by sno asc) t;
		c.select * from (select  rownum r,t.*from (select s.* from student s order by sno asc) t) where r>= (n-1)*10+1and r<=n*10;//oracle 的分页查询语句

	sql server 的分页查询：
		sql server 2003: top
		select top 页面数 * from student where id not in (
					select top (页面数-1)*页面大小 id from student order by sno sec);

		sql server 2010:
		offset fetch next only 

		select * from student order bu sno 
		offset(页数-1)*页面大小-1 fetch next 页面大小 rows only;

```

#### 分页实现：
封装到一个实体类里面
## 5个变量（属性）
1.数据总量
2.页面大小
3.总页数
4.当前页
5.当前页的对象集合（实体类集合）：每页	所显示的所有数据
###### 存放路径
为了防止上传目录丢失 有两种办法 一种是把上传目录放到非tomcat 目录，指定虚拟目录 
# 坑
当你重启服务器的时候你新建的存放文件的文件夹会某明奇妙的消失，这和tomcat 和java 编译的class 有关，有股源文件有修改的话，他会把文件重新部署一边，然而里面的文件就会被重写

## 熟练使用DeBug 进行调试程序
打一个或多个断点
用debug开启服务
F5：跳入方法
F6：向下逐行调试
F7：跳出方法
F8：直接跳转到下一个断点
# 文件下载
//设置消息头
```
		response.addHeader("content-Type", "application/octet-stream");
		response.addHeader("content-Disposition", "attachment;filename=");
		//解决中文文件名乱码
		1.Edge
		+filename++URLEncoder.encode(filename,"utf-8")
		2.Fire Fox
		给文件名加上前缀和后缀
		前缀 =?UTF-8?B?
		Base64.encode
		String 构造方法
		后缀 ?=
		示例：response.addHeader("content-Disposition","attchment;filename==?UTF-8?B?"+new String(Base64.encodeBase64(filename.getBases("UTF-8")))+"?=");
		对于不同的浏览器设置不同的编码格式
		String agent = resquest.getHead("User-Agent");
		if(agent.toLowerCase().indexOff(firefox)){
			response.addHeader("content-Disposition","attchment;filename==?UTF-8?B?"+new String(Base64.encodeBase64(filename.getBases("UTF-8")))+"?=");
		}else{
			response.addHeader("content-Disposition", "attachment;filename="+filename++URLEncoder.encode(filename,"utf-8"));
		}
```
1.Servlet通过文件的地址，转换成输入存放到Servlet
```
InputStream in = getServletContext().getResourceAsStream("/res/MIME.png");
```
2.Servlet把输入流转换成输出流
```
ServletOutputStream out = response.getOutputStream();
```
3.通过转换得到文件
```
byte [] bs = new byte[10];  
		int len = -1;
		while((len = in.read(bs))!=-1) {
			out.write(bs,0,len);
		}
		out.close();
		in.close();
```






