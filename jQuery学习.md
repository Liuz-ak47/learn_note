### jQuery学习

#### 一  jQuery基本

##### 等待dom加载完再执行js代码

``` javascript
$(function() {
    
})
```

$是jQuery的别称，也就是说$可以用jQuery代替，也是jQuery的顶级对象

##### jQuery本质上是对dom对象进行了封装，所以是不一样的，有不同的对象获取方法和api

- DOM对象： var div = document.querySelector("")  使用js原生的api   如 div.style.display = "none"

- jQ对象： 

  - var jq = $("")   应用的是css的元素选择器，如$(".class1") $("#id1") $("span") $("ul>li") $("ul li:nth-child(1)")     相比原生js选取元素的子元素或祖先元素会方便一些，  获取到的是jQuery的对象数组，可以利用jQ的index()方法获得某项的序号，如$("div ul li").index()。
  - 获取到的jQ对象可以直接迭代、链式添加css样式或事件，是全部对象添加。如：$("ul li").css("color", "pink")，即给所有的li添加了字体颜色粉色。若是原生，还得for(var i =0; i < ul.children.length; i++){ul.children[i].style.color = "pink"}
  - 使用jQ特有的api方法， 如 jq.hide()  
  - jQ对象DOM对象转化：jq[index] 或jq.get(index)  index从0开始

- 细数jQ选择器：

  1. 父元素 子元素 兄弟元素的获取方法：

     - $("ul li").parent()  返回亲爸爸

     - $("li").parents() 返回元素的所有祖先 参数中可以选择某个祖先如$("li").parents("div")

     - $("ul").children() 亲儿子选择器，相当于$("ul > li")

     - $("ul").find("p") 子代选择器，所有符合条件的孩子都可以。find也支持比如$("ul").find("p > span")

     - $("ol .item").siblings() 兄弟选择器 ，兄弟中也可以加条件如("li") (".class1") 

     - 筛选操作：

       ```javascript
       $("ul li:first")
       $("ul li:last")
       $("ul li:odd")
       $("ul li:even")
       $("ul li:nth-child(1)")
       $("ul li:eq(" + index + ")")
       $("ul li").eq(index)    //注意eq从0开始，nth-child从1开始
       ```

       

- jQ的类操作
  1. $("ul li").hasClass()   判断是否具有类，返回值true|false
  2. $("div").addClass("current")添加类 不覆盖原来类
  3. $("div").removeClass("current") 删除类 不覆盖原来类
  4. $("div").toggleClass("current")

- jQ排他思想，链式编程

  ```javascript
  $(function(){
      $("button").on("click",function(){
          $(this).css("backgroundColor", "pink").siblings("button").css("backgroundColor", "")
      })
  })
  //this触发事件的元素，$("button")是绑定事件的元素
  ```

- jQ的css样式操作方法

  1. 获取某个css样式属性的值

     ```
     $("div").css("width")  //获取到比如  ”400px“
     ```

  2. 设置单个css样式

     ```
     $("div").css("width", "300px")	
     $("div").css("width", 300)	//或者这样
     ```

  3. 设置多个css样式

     ```
     $("div").css({
     	width: 400
     	hight: 400
     	backgroundColor: "red"
     })
     ```

#### 二 jQuery动画

##### 显示与隐藏--直接   实质是display: block/none

```javascript
$("div").show()
$("div").hide()
$("div").show(1000, function(){})  //显示1s后执行回调函数，显示的动画效果是1s
$("div").hide(1000, function(){})
$("div").toggle(1000) //注意 jQ对象.toggle()方法是显示与隐藏
```

##### 显示与隐藏元素--滑动效果

```javascript
$("div").slideUp() //隐藏
$("div").slideDown() //显示
$("div").slideUp(1000, function(){})
$("div").slideDown(1000, function(){})
$("div").slideDown(1000, function(){})
$("div").slideDown(1000, function(){})
$("div").slideToggle(1000)
$("div").slideToggle(1000, function(){}) // jQ对象.slideToggle()是以滑动切换和隐藏
```

##### 显示与隐藏元素--淡入淡出效果

```javascript
$("div").fadeIn()
$("div").fadeOut()  //参数同上，可以加时间和回调函数
$("div").fadeToggle() //jQ对象以淡入淡出效果切换隐藏和显示
$("div").fadeTo(1000, 0.5) //时间内切换透明度
```

ps: 可以用.stop()方法停止动画排队，如

```javascript
$("div").stop().fadeToggle() //stop 方法必须写到动画的前面
```

##### 自定义动画animate

```javascript
$("div").animate({
	left: 400,
	width: 500,
	opacity: .5,
}, 500)
```

```
$("body, html").stop().animate({
                    scrollTop: 0
       			});
```



#### 三 jQ事件

##### 触发事件的几种写法：

1. 元素.事件

```
$(".nav>li").mouseover(function(){})
$(".nav>li").mouseout(function(){})
```

2. 复合写法，hover就是上面两个事件的复合	

   ```javascript
   $(".nav>li").hover(
       function(){$(this).children("ul").slideDown(200)}, 
       function(){$(this).children("ul").slideUp(200)}
   )
   $(".nav>li").hover(function(){
       $(this).children("ul").slideToggle(200)
   }) //如果只写一个函数，则鼠标经过和离开都会触发这个函数
   ```

3. 事件处理on

   - 给一个元素绑定事件

     ```javascript
     $("div").on("click", function(){}) //相当于 $("div").click(function(){})
     ```

   - 可以同一个元素绑定多个事件

     ```
     $("div").on({
     	click: function(){
     		$(this).css("backgroundColor", "red")
     	},
     	mouseover: function(){
     		$(this).css("backgroundColor", "red")
     	}
     })
     或者这么写
     $("div").on("mouseenter, mouseover", function(){
     	$(this).toggleClass("current")
     })
     ```

   - 实现事件委托

     ```
     $("ul").on("click", "li", function(){$(this).css("color", "red")})
     //详细说明： 是给ul绑定的事件，但是触发事件的对象是每个li，this指代的触发事件的li
     ```

   - 可以操作未来创建的元素

     ```
     $("ol").on("click", "li", function(){
     	console.log($(this)[0]) //给ol绑定事件时，还没有li，但是click触发时，this指向的是li
     })								
     var li = $("<li>我是后来创建的元素<li>")
     $("ol").prepend(li)
     ```

4. 只能触发一次的事件

   ```
   $("div").one("click", function(){})
   ```

##### 事件解绑

```
$("div").off()   //解除对象的全部事件
$("div").off("click")  //解除对象的指定事件
$("div").off("click"，"li")
```

##### 自动触发事件

```
$("input").focus()
$("div").click()
或通过trigger
$("input").trigger("focus")
```

##### 事件对象

```
$("div").click(function(event){
	console.log($(this)[0])  //触发事件的对象
	console.log(event)  //鼠标点击事件
	event.stopPropagation()  //阻止事件冒泡
})
```



#### 四 jQ属性操作

1. 元素固有属性值的获取和设置

   ```javascript
   $("input").prop("type") //获取
   $("a").prop("title", "都挺好")  //设置
   ```

2. 元素自定义属性值的获取和设置

   ```javascript
   $("div").attr("index")
   $("div").attr("index", "4")
   ```

3. 获取h5自定义属性如data-index = "3"

   ``` javascript
   $("div").data("index")  //只能获取不能设置
   ```

4. 元素属性缓存 

   ```
   $("div").data("uname", "andy") //设置
   $("div").data("uname") //获取
   ```

   

#### 五 jQ元素内容

##### jQ元素内容

1. 元素内容的获取和设置，可以处理标签

   ```
   $("div").html()  //获取div标签内的内容，包含标签和文本内容，以字符串形式表达
   $("div").html(“123” + "<a href='javascript:;'>删除</a>") //设置div内容是 123删除，以标签渲染a
   ```

2. 元素文本内容的获取和设置

   ```
   $("div").text()
   $("div").text("123")
   ```

3. 获取和设置表单值，即value

   ```
   $("input").val()
   $("input").val("123")
   ```

##### jQ元素对象的创建 添加和删除

1. 创建

   ```javascript
   var li = $("<li>我是创建的元素</li>")  //可以解析标签
   ```

2. 添加

   - 指定对象内部添加

     ```
     $("div").prepend(li) //div内部最前添加
     $("div").append(li) //div内部最后添加
     ```

   - 指定对象平级添加

     ```
     $(".test").after(li)  //.test对象后添加
     $(".test").before(li)  //.test对象前面添加
     ```

3. 删除

   ```
   $("li").remove()  删除自身
   $("ul").empty()  删除内部全部元素
   $("ul").html("")  通过设置元素内容方式清空内容
   ```

   

#### 六 jQ遍历 

	##### jQ.each()方法

1. 遍历jQ对象，进行操作。

   如下是获取所有的div元素，进行操作。回调函数中第一个参数是索引号，第二个参数是遍历到的dom对象，还需转化为jQ对象

   ```
   <div>1</div>
   <div>2</div>
   var arr = ["red", "blue", "pink"]
   $.each($("div"), function(index, docele){
   	$("docele").css("color", arr[index])
   })
   ```

2. 遍历普通数组，进行操作

   ```
   var arr = ["red", "blue", "pink", 1]
   $.each(arr, function(index, ele){
   	console.log(index)  //0 1 2 3 数组下标
   	console.log(ele)  // "red" "blue" "pink" 1 数组每项值
   })
   ```

   

3. 遍历普通对象，进行操作

   ```
   var obj = {
   	name: "andy",
   	age: 18
   }
   $.each(obj, function(key, value){
   	console.log(key)   //对象的键，字符串
   	console.log(value) //对象的值
   })
   ```

   

#### 七 元素的大小 位置 卷去部分

##### 获取和设置大小

```javascript
//content的宽度、高度
$("div").width()  // 200 数字不带单位，number类型
$("div").width(400)
//content + padding的宽度、高度
$("div").innerWidth()
$("div").innerWidth(400)
//content + padding + border
$("div").outerWidth()
$("div").outerWidth(400)
//content + padding + border+ margin
$("div").outerWidth(true)
$("div").outerWidth(true, 400)
```

##### 获取和设置位置

1. 获取和设置距离**文档document**的值

   - 获取

   ```
   $("div").offset()  //得到比如{top: 200, left: 100}
   $("div").offset().top  //得到比如 200
   $("div").offset().left  //得到比如 100
   ```

   - 设置

     ```
     $("div").offset({
     	top: 200,
     	left: 400
     })
     ```

2. 获取具有定位的父级元素的距离，只能获取不能设置。如父级没有定位，则获得距离文档的距离。

   ```
   $("div").position()  //得到比如{top: 200, left: 100}
   ```

##### 页面被卷去的部分

```
$("document").scrollTop() //得到数值 比如100
$("document").scrollLeft() //得到数值 比如100
$("document").scrollTop(0) //使页面滚动到顶部
$("document").scrollLeft(0) 
$("body, html").stop().animate({
                    scrollTop: 0
       			});
```



#### 八 jQ对象拷贝

1. 浅拷贝  拷贝地址，修改新对象，旧对象也会改变。

   ```
   $.extend(newObj, oldObj)   //会将旧对象的复杂数据类型地址拷贝给目标对象。键名相同时覆盖，保留newObj中原有键值
   ```

2. 深拷贝  拷贝对象，修改新对象，旧对象不会改变。

   ```
   $extend(ture, newObj, oldObj)  //会拷贝对象值到新对象中。同样属性不冲突时保留newObj原有键值
   ```

#### 九 多库共存

```
$(function() {
            function $(ele) {
                return document.querySelector(ele);
            }
            console.log($("div"));
            // 1. 如果$ 符号冲突 我们就使用 jQuery
            jQuery.each();
            // 2. 让jquery 释放对$ 控制权 让用自己决定
            var suibian = jQuery.noConflict();
            console.log(suibian("span"));
            suibian.each();
        })
```

