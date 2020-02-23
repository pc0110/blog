---
title: Ajax
date: 2019-10-18 09:50:04
tags: Jsp
---
# 异步刷新 js和xml
异步刷新：如果网页中的某一个地方需要修改，异步刷新可以使：只刷新需要修改的地方，而页面其他的地方保持不变 
例如：百度搜索框

### 实现：


### jquery:(常用)


###### ajax

```

$.ajax({
url:服务器地址,
请求方式:post|get,
data：请求的数据,
success:function(result,testStatus){

},
error:function(xhr,errorMessage,e){
        
}

});

```
###### 第二种方式 get post load


$.get(
服务器地址,
请求数据,
function (result){

}
)
$.post(
服务器地址,
请求数据,
function (result){
        
},
预期返回值类型(string\xml) -> "text"   "json"  "xml"
);

将服务端的返回值，返回到load选择的元素中("#tip")
$("#tip").load(
服务器地址,
请求数据,

把返回值放到Servlet中去  用<span id = "tip"></span>显示
);
# json 数组
var student = {"name":"name","sno":12}
var students = [
        {"name":"name","sno":12}
        {"name":"name","sno":122}
        {"name":"name","sno":1222}
]
alter(studnets[1].name+"----"+students.sno[1]);

###### $.getJSON()
$.getJSON(
服务器地址,
json格式的请求数据,

```
function (result){
        if(result.msg=="true"){//msg:true|false 
				alert("请更换号码，此号码已存在");
			}else{
				alert("注册成功");
				
			}
}
)
```
如果前端的返回值是json格式 服务端传值也要是json
out.write("{\"msg\":"\"true\"}");   write只有一个参数  ，里面的引号 通过转义字符显示





### js:  XMLHttpRequest
```
XMlHttpRequest对象的方法：
Open(方法名(提交方式get|post),服务器地址，ture):与服务器建立连接   
send():
    get: send(null)
    post: send(参数值))
setRquestHeader(header,value):
    get:不需要设置此方法
    post:需要设置：
            a.如果请求的元素中包含了上传文件：
                    setRequestHeader("Content-Type","multipart/from-data");
            b.不包含了上传文件
                    setRequestHeader("Content-Type","application/x-www-form-urlencoded");

```

### XMLHttpRquest对象的属性：
readyState:请求状态  0-4 只有状态为4 代表请求完毕
status:响应状态     只有200代表响应正常
onreadystatechange: 回调函数
responseText:响应格式为String
responseXML:响应格式为XML








