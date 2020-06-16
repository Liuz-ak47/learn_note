### Ajax技术



#### Ajax简述

概括： 使用js代码异步获取服务器同源数据

**目的**： 获取服务器同源数据、

**特点**： 局部刷新（不需要url改变全局重新加载页面）、异步加载（加载时页面其他部分仍然可以操作）

 

#### 原生Ajax基本使用步骤

1. 创建xhr对象
2. open方法准备发送
3. send方法执行发送
4. 回调函数得到返回值

#### 原生Ajax的封装

```javascript
//传入的参数是一个对象形式，如
obj = {                 
    url: "./application.php",                   //请求地址
    type: "post",								//请求方式
    data: {uname: "zhangsan", age: "18"},		//请求参数
    success: function(result) {					//回调函数

    }
}

//Ajax函数封装
function myAjax(obj) {
    // 默认的参数
    var defaults = {
        url: "#",
        type: "get",
        data: {},
        dataType: "json",
        async: true,
        success: function(result) {console.log(result)}
    }

    // 传参obj与默认参数合并
    for(var key in obj){
        defaults[key] = obj[key]
    }

    // 原生ajax的四个步骤
    // 1.新建ajax对象
    var xhr = null
    if(window.XMLHttpRequest) {
        xhr = new XMLHttpRequest()
    } else {
        xhr = new ActiveXObject("Microsoft.XMLHTTP")
    }

    // 2.设置参数
    var params = "";
    for(var attr in defaults.data) {
        params += attr + "=" + defaults.data[attr] + "&"
    }
    if (params) {
        params = params.substring(0, params.length - 1)
    }
    if(defaults.type == "get") {
        defaults.url += "?" + params
    } 
    xhr.open(defaults.type, defaults.url, defaults.async)

    // 3.发送请求
    if(defaults.type == "get") {
        xhr.send(null)
    } else if(defaults.type == "post") {
        xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")
        xhr.send(params)
    }

    // 4.请求的结果执行回调函数

    if(defaults.async == true) {
        xhr.onreadystatechange = function() {
            if(xhr.readyState == 4) {
                if (xhr.status == 200) {
                    var result = null
                    if(dataType == "json") {
                        result = xhr.responseText
                        result = JSON.parse(result)
                    } else if(dataType == "XML") {
                        result = xhr.responseXML
                    } else {
                        result = xhr.responseText
                    }
                    defaults.success(result)
                }
            }
        }
    } else if(defaults.async == false) {
        if(xhr.readyState == 4) {
            if (xhr.status == 200) {
                var result = null
                if(dataType == "json") {
                    result = xhr.responseText
                    result = JSON.parse(result)
                } else if(dataType == "XML") {
                    result = xhr.responseXML
                } else {
                    result = xhr.responseText
                }
                defaults.success(result)
            }
        }
    }
}

```



#### JQuery封装Ajax的使用

##### 形式1 $.ajax()    最常用的形式

```javascript
$.ajax({
    url: "",
    type: "get",     /"post"
    data: {},
    dataType: "json",   /"text" /"xml" 代表返回数据的格式。这几个值的话，进行同源请求，进行上面的步骤1234
    async: true,       /false
    success: function(result){console.log(result)  json格式的别忘了JSON.parse(result)}
})
```

##### 形式2 $.get()

```
$.get(url + "?" + params, function(result){console.log(result)})
```

##### 形式3 $.post()

```
$.post(url, {}, function(result){console.log(result)})
```



#### 跨域的概念及解决方案

跨域的概念是源于浏览器的同源策略，是一种安全机制。ajax默认只能获取到与服务器同源的数据。

**简单来说，Ajax的目的是访问同源的数据，跨域的目的是访问别人的数据**

注：即在服务器端请求到的页面中，比如进行用户名密码邮箱校验时，还要发送异步或同步请求数据到另一个地址进行校验，此时这个地址就会和服务器地址产生跨域。

##### 什么是同源地址？

即协议名，域名，端口号都相同的地址。

如下面两个地址是同源的

http://www.example.com/page1.html

http://www.example.com/index/page2.html

##### 跨域解决方案 之jsonp方案

1. jsonp解决方案：利用script标签的src属性传递地址、参数和**回调函数**，获得外部文件的接口数据的**方法调用**。因为是src，必然是get方法。

   **请注意：返回的数据格式一定是jsonp格式，一定是方法调用，否则，无法正常获得数据**。如果返回数据是json格式，另有由服务器端做代理的方法实现，我们不需要太过了解。

   
   
   **简洁版代码**
   
   前端代码：

```javascript
window.onload = function() {
            $("#btn").on('click', function(){
                var city = document.querySelector("#city");
                var cityValue = city.value;
                var script = document.createElement('script');
                script.src = "http://www.zhangsan.com/test.php?city=" + cityValue + "&" + "callback=foo2";
                window['foo2'] = function(result){
                    console.log("我是foo2方法的输出" + result);
                }
                var head = document.querySelector("head");
                head.appendChild(script);
            })
        }
```

​	后台php代码：

```php
<?php
    // 动态获取回调函数名称的代码
    $city = $_GET["city"];
    $cbName = $_GET["callback"];    //$cbName就是foo2
    if($city == "beijing") {
        echo $cbName."('北京的天气晴')";   //注意php语法使用.来连接字符串
    } else {
        echo $cbName."('未查询到该地区')";
    }
?>
```



​	**详细的加注释代码**  详情见desktop/myweb/test.html

​	前端代码

```javascript
<h2>这一个大标题</h2>
    <input type="text" name="" id="city" placeholder="请输入查询的城市">
    <input type="button" value="查询" id="btn">
    <script> 
        // 同源策略：在www.liuzhe.com跨域访问不到张三的数据
        // var url = "http://www.zhangsan.com"
        // $.get(url, function(result){
        //     console.log(result);
        // })  
    </script>



    <!-- 解决跨域方式1： jsonp的方式，利用script标签的src属性传递地址、参数和回调函数，获得外部文件的接口数据和执行回调函数（文件返回的数据是js类型的） -->
    <!-- 预先定义了foo2方法 -->
    <script>
        // function foo2(params){
        //     console.log("我是foo2方法的输出" + params);
        // }
    </script>
    <!-- script标签引入test.php文件 -->
    <!-- <script type="text/javascript" src="http://www.zhangsan.com/test.php?city=beijing"></script> -->
    <!-- script标签引入kuayu.js文件 -->
    <script type="text/javascript" src="http://www.zhangsan.com/kuayu.js">
        // console.log(str);  这里获得不了str不知道为什么，下面的script标签才能获取到str
    </script>
    <script>
        //这是kuayu.js文件的实验
        //在服务器端预先定义方法调用
        function foo(params) {
            console.log(params);
        }
        // 利用jsonp的方式返回的接口数据，进行处理
        foo(str);   //输出"bigdaddy"

        // 这是test.php文件的实验
        //输出 "我是foo2方法的输出beijing的天气晴"

        // 动态传参：   动态创建script标签拼接src参数
        window.onload = function() {
            $("#btn").on('click', function(){
                var city = document.querySelector("#city");
                var cityValue = city.value;
                var script = document.createElement('script');
                // script.src = "http://www.zhangsan.com/test.php?city=" + cityValue; 这种不将回调函数名称传递到后台的方式，前后台回调函数名字必须一致，我们想要的效果是前端可以任意指定回调函数名称，后台都可以执行。所以使用下面方式，将回调函数名称也当做参数传递到后台
                script.src = "http://www.zhangsan.com/test.php?city=" + cityValue + "&" + "callback=foo2"
                window['foo2'] = function(params){
                    console.log("我是foo2方法的输出" + params);
                }
                var head = document.querySelector("head");
                head.appendChild(script);
            })
        }
```

​	后台php代码：

```php
<?php

    //没有动态获取回调函数名称的代码
    // $city = $_GET["city"];
    // if($city == "beijing") {
    //     echo "foo2('beijing的天气晴')";
    // } else {
    //     echo "foo2('没有查询到该地区')";
    // }
    
    // 动态获取回调函数名称的代码
    $city = $_GET["city"];
    $cbName = $_GET["callback"];
    if($city == "beijing") {
        echo $cbName."('北京的天气晴')";   //注意php语法使用.来连接字符串
    } else {
        echo $cbName."('未查询到该地区')";
    }

    // echo "foo2('123')";

?>
```

##### 自己封装jsonp

```javascript
//传入的而是一个对象，如：
obj = {
    type: "get",   //对跨域来说这个填不填无所谓，只能是get
    url: "www.baidu.com",
    data: {uname: "lili", age: 56}, //重要的，要看接口文档中参数名是啥
    jsonp: "callback",    //这个重要的，是接口文档中指定的回调函数key值
    success: function(data){
        // do something   //对得到的结果进行的业务逻辑处理
    }
}

//jsonp的封装
function myAjax(obj){
            var defaults = {
                type: "get",
                url: "#",
                data: {},
                jsonp: "callback",  //接口文档中指定的传入后台的函数调用的key值
                jsonpCallback: "hehe",   //这个无所谓，，接口文档中指定的函数调用的value名字，即?callback=啥
                success: function(data){   //这里是要对获取到的数据进行处理的函数
                    console.log(data);
                }
            }
            for(var key in obj){
                defaults[key] = obj[key];
            }

            var params = "";
            for(var attr in defaults.data){
                params += attr + "=" + defaults.data[attr] + "&" 
            }
            if(params){
                params = params.substring(0, params.length - 1)
            }
            defaults.url += "?" + params + "&" + defaults.jsonp + "=" + defaults.jsonpCallback;
            var script = document.createElement("script");
            script.src = defaults.url;

            window[defaults.jsonpCallback] = function(data){
                defaults.success(data);
            }

            var head = document.querySelector("head");
            head.appendChild(script);
        }
```

##### jQuery封装的跨域jsonp方法

```javascript
$.ajax({
    url: "http://suggest.taobao.com/sug",
    data: {q: keyValue},   //根据接口文档确定参数键
    dataType: "jsonp",   //注意 jsonp是jQuery实现跨域关键，跨域标志,进行跨域请求动态创建script标签获取非同源数据，而不进行ajax1234同源请求的的步骤
    jsonp: "callback",  //jQuery中传入到后台的函数调用，默认key值是callback，函数名是以jQuery开头的一个随机值字符串, 分别可以用jsonp和jsonpCallback修改
    jsonpCallback: "heihei"
    async: true,       
    success: function(result){console.log(result)}
})

```

#### 综合的可以适用于跨域或同源的方法

**请注意：上面不论是jQuery封装的还是自己封装的，返回的数据格式一定是jsonp格式，一定是方法调用，否则，无法正常获得数据**

##### jQuery封装的ajax：  形式上与上面两种一样，关键在于dataType的值是不是jsonp

```
$.ajax({
    url: "http://suggest.taobao.com/sug",
    data: {q: keyValue},   //根据接口文档确定参数键
    dataType: "jsonp",   //注意 jsonp是jQuery实现跨域关键，跨域标志,进行跨域请求动态创建script标签获取非同源数据，而不进行ajax1234同源请求的的步骤
    jsonp: "callback",  //jQuery中传入到后台的函数调用，默认key值是callback，函数名是以jQuery开头的一个随机值字符串, 分别可以用jsonp和jsonpCallback修改
    jsonpCallback: "heihei"
    async: true,       
    success: function(result){console.log(result) dataType为json格式的注意转化为js对象}
})
```

##### 自己封装的，可以既用于跨域，又用于同源。 

其实就是根据dataType加了一个判断，将自己封装的同源和跨域综合了

```javascript
        function myAjax(obj) {
            if(obj.dataType == "jsonp") {
                myAjax4Across(obj);
            } else {
                myAjax4Normal(obj);
            }

            // 同源实现： 创建XHR对象，1234
            function myAjax4Normal(obj) {
            // 默认的参数
            var defaults = {
                url: "#",
                type: "get",
                data: {},
                dataType: "json",
                async: true,
                success: function(result) {console.log(result)}
            }

            // 传参obj与默认参数合并
            for(var key in obj){
                defaults[key] = obj[key]
            }

            // 原生ajax的四个步骤
            // 1.新建ajax对象
            var xhr = null
            if(window.XMLHttpRequest) {
                xhr = new XMLHttpRequest()
            } else {
                xhr = new ActiveXObject("Microsoft.XMLHTTP")
            }

            // 2.设置参数
            var params = "";
            for(var attr in defaults.data) {
                params += attr + "=" + defaults.data[attr] + "&"
            }
            if(params) {
                params = params.substring(0, params.length - 1)
            }
            if(defaults.type == "get") {
                defaults.url += "?" + params
            } 
            xhr.open(defaults.type, defaults.url, defaults.async)

            // 3.发送请求
            if(defaults.type == "get") {
                xhr.send(null)
            } else if(defaults.type == "post") {
                xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")
                xhr.send(params)
            }

            // 4.请求的结果执行回调函数

            if(defaults.async == true) {
                xhr.onreadystatechange = function() {
                    if(xhr.readyState == 4) {
                        if (xhr.status == 200) {
                            var result = null
                            if(dataType == "json") {
                                result = xhr.responseText
                                result = JSON.parse(result)
                            } else if(dataType == "XML") {
                                result = xhr.responseXML
                            } else {
                                result = xhr.responseText
                            }
                            defaults.success(result)
                        }
                    }
                }
            } else if(defaults.async == false) {
                if(xhr.readyState == 4) {
                    if (xhr.status == 200) {
                        var result = null
                        if(dataType == "json") {
                            result = xhr.responseText
                            result = JSON.parse(result)
                        } else if(dataType == "XML") {
                            result = xhr.responseXML
                        } else {
                            result = xhr.responseText
                        }
                        defaults.success(result)
                    }
                }
            }
        }

        // 跨域实现： 动态创建script标签
        function myAjax4Across(obj){
            var defaults = {
                type: "get",
                url: "#",
                data: {},
                jsonp: "callback",  //接口文档中指定的回调函数的key值
                jsonpCallback: "hehe",   //这个无所谓，，接口文档中指定的回调函数的value名字，即?callback=啥
                success: function(data){   //这里是要对获取到的数据进行处理的函数
                    console.log(data);
                }
            }
            for(var key in obj){
                defaults[key] = obj[key];
            }

            var params = "";
            for(var attr in defaults.data){
                params += attr + "=" + defaults.data[attr] + "&" 
            }
            if(params){
                params = params.substring(0, params.length - 1)
            }
            defaults.url += "?" + params + "&" + defaults.jsonp + "=" + defaults.jsonpCallback;
            var script = document.createElement("script");
            script.src = defaults.url;

            window[defaults.jsonpCallback] = function(results){
                defaults.success(data);
            }

            var head = document.querySelector("head");
            head.appendChild(script);
        }
    }
```

#### 模板引擎

目的：ajax获取数据之后，为了不再使用循环手工拼接标签的形式渲染数据，和为了方便阅读和维护，使用模板引擎。

##### 基本使用方式

```javasct
1. 引入template.js文件
2. 建立入script标签，注意类型是text/html，且要给模板加一个id。例如下面（比如ajax返回的数据是data对象，其中有属性为s值是数组。那么下面模板i就是数组的下标值，value就是数组中每一项）
	<script type="text/html" id="resultTemplate">
		{{ifture}}
		{{str}}
		<ul>
			{{each s as value i}}
			<li>
				<span>结果{{i}}</span>
				<span>{{value}}</span>
			</li>
			{{/each}}
		</ul>
		{{ifture}}
	</script>
3. body中使用template方法，将模板和ajax返回的数据结合起来，成html片段。如： //data是ajax返回的对象
	var html = tempalte("resultTempla", data)
4. 在某个地方使用这个html片段， 如：
	var div1 = document.querySelector("#div1")
	div1.innerHTML = html
	
ps：
	可以加条件判断{{ifture}}{{/ifture}}，比如上面只有在ifture为ture的时候，模板才会生成
	data = {
		str: "<span style='color: red;'>四大名著</span>"
		s: ['三国演义', '红楼梦', '水浒传', '西游记']
	}
    {{each s as value i}}遍历对象的s数组              
	{{str}}是获取到对象的某个属性的值，默认会进行转义解析，渲染成字符串形式："<span style='color: red;'>四大名著</span>"，如果需要进行不转义解析，即使标签有效果，加#号 {{#str}}
	有时需要对数据进行处理，处理成对象形式，这样才好用{{属性名}}的方式得到值
```

