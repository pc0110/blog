---
title: Ajax
date: 2019-10-18 09:50:04
<<<<<<< HEAD
tags:
-JavaScript
---
# Ajax

常见的方法

**1.url**: 
要求为String类型的参数，（默认为当前页地址）发送请求的地址。

**2.type**: 
要求为String类型的参数，请求方式（post或get）默认为get。注意其他http请求方法，例如put和delete也可以使用，但仅部分浏览器支持。

**3.timeout**: 
要求为Number类型的参数，设置请求超时时间（毫秒）。此设置将覆盖$.ajaxSetup()方法的全局设置。

**4.async**: 
要求为Boolean类型的参数，默认设置为true，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为false。注意，同步请求将锁住浏览器，用户其他操作必须等待请求完成才可以执行。

**5.cache**: 
要求为Boolean类型的参数，默认为true（当dataType为script时，默认为false），设置为false将不会从浏览器缓存中加载请求信息。

**6.data**: 
要求为Object或String类型的参数，发送到服务器的数据。如果已经不是字符串，将自动转换为字符串格式。get请求中将附加在url后。防止这种自动转换，可以查看　　processData选项。对象必须为key/value格式，例如{foo1:"bar1",foo2:"bar2"}转换为&foo1=bar1&foo2=bar2。如果是数组，JQuery将自动为不同值对应同一个名称。例如{foo:["bar1","bar2"]}转换为&foo=bar1&foo=bar2。

**7.dataType**: 
要求为String类型的参数，预期服务器返回的数据类型。如果不指定，JQuery将自动根据http包mime信息返回responseXML或responseText，并作为回调函数参数传递。可用的类型如下：
xml：返回XML文档，可用JQuery处理。
html：返回纯文本HTML信息；包含的script标签会在插入DOM时执行。
script：返回纯文本JavaScript代码。不会自动缓存结果。除非设置了cache参数。注意在远程请求时（不在同一个域下），所有post请求都将转为get请求。
json：返回JSON数据。
jsonp：JSONP格式。使用SONP形式调用函数时，例如myurl?callback=?，JQuery将自动替换后一个“?”为正确的函数名，以执行回调函数。
text：返回纯文本字符串。

**8.beforeSend**：
要求为Function类型的参数，发送请求前可以修改XMLHttpRequest对象的函数，例如添加自定义HTTP头。在beforeSend中如果返回false可以取消本次ajax请求。XMLHttpRequest对象是惟一的参数。
      function(XMLHttpRequest){
        this;  //调用本次ajax请求时传递的options参数
      }
**9.complete**：
要求为Function类型的参数，请求完成后调用的回调函数（请求成功或失败时均调用）。参数：XMLHttpRequest对象和一个描述成功请求类型的字符串。
     function(XMLHttpRequest, textStatus){
       this;  //调用本次ajax请求时传递的options参数
     }

**10.success**：要求为Function类型的参数，请求成功后调用的回调函数，有两个参数。
     (1)由服务器返回，并根据dataType参数进行处理后的数据。
     (2)描述状态的字符串。
     function(data, textStatus){
      //data可能是xmlDoc、jsonObj、html、text等等
      this; //调用本次ajax请求时传递的options参数
     }

**11.error**:
要求为Function类型的参数，请求失败时被调用的函数。该函数有3个参数，即XMLHttpRequest对象、错误信息、捕获的错误对象(可选)。ajax事件函数如下：
    function(XMLHttpRequest, textStatus, errorThrown){
     //通常情况下textStatus和errorThrown只有其中一个包含信息
     this;  //调用本次ajax请求时传递的options参数
    }

**12.contentType**：
要求为String类型的参数，当发送信息至服务器时，内容编码类型默认为"application/x-www-form-urlencoded"。该默认值适合大多数应用场合。

**13.dataFilter**：
要求为Function类型的参数，给Ajax返回的原始数据进行预处理的函数。提供data和type两个参数。data是Ajax返回的原始数据，type是调用jQuery.ajax时提供的dataType参数。函数返回的值将由jQuery进一步处理。
      function(data, type){
        //返回处理后的数据
        return data;
      }

**14.dataFilter**：
要求为Function类型的参数，给Ajax返回的原始数据进行预处理的函数。提供data和type两个参数。data是Ajax返回的原始数据，type是调用jQuery.ajax时提供的dataType参数。函数返回的值将由jQuery进一步处理。
      function(data, type){
        //返回处理后的数据
        return data;
      }

**15.global**：
要求为Boolean类型的参数，默认为true。表示是否触发全局ajax事件。设置为false将不会触发全局ajax事件，ajaxStart或ajaxStop可用于控制各种ajax事件。

**16.ifModified**：
要求为Boolean类型的参数，默认为false。仅在服务器数据改变时获取新数据。服务器数据改变判断的依据是Last-Modified头信息。默认值是false，即忽略头信息。

**17.jsonp**：
要求为String类型的参数，在一个jsonp请求中重写回调函数的名字。该值用来替代在"callback=?"这种GET或POST请求中URL参数里的"callback"部分，例如{jsonp:'onJsonPLoad'}会导致将"onJsonPLoad=?"传给服务器。

**18.username**：
要求为String类型的参数，用于响应HTTP访问认证请求的用户名。

**19.password**：
要求为String类型的参数，用于响应HTTP访问认证请求的密码。

**20.processData**：
要求为Boolean类型的参数，默认为true。默认情况下，发送的数据将被转换为对象（从技术角度来讲并非字符串）以配合默认内容类型"application/x-www-form-urlencoded"。如果要发送DOM树信息或者其他不希望转换的信息，请设置为false。

**21.scriptCharset**：
要求为String类型的参数，只有当请求时dataType为"jsonp"或者"script"，并且type是GET时才会用于强制修改字符集(charset)。通常在本地和远程的内容编码不同时使用。

案例代码：

```javascript
$(function(){
    $('#send').click(function(){
         $.ajax({
             type: "GET",
             url: "test.json",
             data: {username:$("#username").val(), content:$("#content").val()},
             dataType: "json",
             success: function(data){
                         $('#resText').empty();   //清空resText里面的所有内容
                         var html = ''; 
                         $.each(data, function(commentIndex, comment){
                               html += '<div class="comment"><h6>' + comment['username']
                                         + ':</h6><p class="para"' + comment['content']
                                         + '</p></div>';
                         });
                         $('#resText').html(html);
                      }
         });
    });
});
```

# 异步刷新 js和xml

=======
tags: Jsp
---
# 异步刷新 js和xml
>>>>>>> e38011719beca8f9c3c9ce58b48689d26f37629e
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








