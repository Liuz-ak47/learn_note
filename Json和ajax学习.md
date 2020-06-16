# Json和Ajax学习

> 什么是JSON？

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

```
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

**JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。**

```
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```



**JSON 和 JavaScript 对象互转**

要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法：

```
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
```

要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法：

```
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```

```
var obj = {
            name: 'JJ',
            age: 18,
            sex: 'male'
        }
        var jsonStr = JSON.stringify(obj);
        console.log(jsonStr);  //'{"name":"JJ","age":18,"sex":"male"}' 字符串
        var jsonObj = JSON.parse(jsonStr);
        console.log(jsonObj); //{name: "JJ", age: 18, sex: "male"} 对象
        var arr = JSON.parse('[1, 2]')
        console.log(arr); //[1, 2] 数组
```



#### Json解析工具

JsonView

#### 网络模块选择

#### 什么是ajax

> AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。
>
> Ajax的核心是XMLHttpRequest对象(XHR)。XHR为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

1. jQuery-Ajax

   - 传统Ajax基于XMLHttpRequest, 混乱，实际开发很少用，所以选择jQuery-Ajax

   - 使用jQuery-Ajax需要引用jQuery，同时存在回调地狱。

   - 通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。

   - 跨域解决方式参见：[ajax跨域](https://segmentfault.com/a/1190000012469713)
   
     > ```
     > jQuery.ajax(...)
     >    部分参数：
     >          url：请求地址
     >          type：请求方式，GET、POST（1.9.0之后用method）
     >      headers：请求头
     >          data：要发送的数据, 对象形式
     >  contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
     >        async：是否异步
     >      timeout：设置请求超时时间（毫秒）
     >    beforeSend：发送请求前执行的函数(全局)
     >      complete：完成之后执行的回调函数(全局)
     >      success：成功之后执行的回调函数(全局)
     >        error：失败之后执行的回调函数(全局)
     >      accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
     >      dataType：将服务器端返回的数据转换成指定类型
     >        "xml": 将服务器端返回的内容转换成xml格式
     >        "text": 将服务器端返回的内容转换成普通文本格式
     >        "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
     >      "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
     >        "json": 将服务器端返回的内容转换成相应的JavaScript对象
     >      "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 callback 为正确的函数名，以执行回调函数
     >      
     >      <html>
     > <head>
     > <title>$Title$</title>
     > <%--<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>--%>
     > <script src="${pageContext.request.contextPath}/statics/js/jquery-3.1.1.min.js"></script>
     > <script>
     >     function a1(){
     >         $.ajax({
     >             url:"${pageContext.request.contextPath}/a1",
     >             type: 'post'
     >             dataType: 'JSON'
     >             data:{'name':$("#txtName").val()},
     >             success:function (data,status) {
     >                 alert(data);
     >                 alert(status);
     >            }
     >        });
     >    }
     > </script>
     > </head>
     > <body>
     > 
     > <%--onblur：失去焦点触发事件--%>
     > 用户名:<input type="text" id="txtName" onblur="a1()"/>
     > 
     > </body>
     > </html>
     > ```

2. axios 

   - 本质上也是基于XHR，在浏览器中创建XHR

   - 支持Promise API，在node js中发送http请求

   - 拦截请求和响应

   - 跨域解决方式：[axios跨域](https://blog.csdn.net/wh_xmy/article/details/87705840)

     ```
     axios({
       url: 'http://123.207.32.32:8000/home/data',
       // url: 'http://123.207.32.32:8000/home/data?type=sell&page=1'
       // 针对get请求，参数可以放到param里，axios会自动进行拼接
       params: {
         type: 'pop',
         page: 1
       }
     }).then(res => {
       console.log(res);
     })
     
     axios.all([axios({
       url: '/home/data?type=sell&page=1'
     }), axios({
       url: '/home/data',
       params: {
         type: 'sell',
         page: 5
       }
     })]).then(axios.spread((res1, res2) => {
       console.log(res1);
       console.log(res2);
     }))
     ```

     



