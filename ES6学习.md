#### 红J3

4看完第三章（除了Symbol）

开始看3，第四章 + ES6入门，完事再看红J4

5.2 ARRAY

#### ES6学习

##### let、作用域等 相关、for循环、function

###### for循环

```javascript
            // i只在循环体内部生效，相当于for的括号中的i是1.5层的块级作用域，花括号是2层块级作用域。如果不重新声名2层作用域的i，会按照作用域链规则向1.5层查找，当重新声名2层的i时，会使用它自己的。当修改2层作用域的i，不会影响循环的控制(因为是1.5层的i控制)。在外部(1层作用域，最外面的作用域)访问i时是访问不到的，因为外部无法访问内部。
            // 如果循环控制使用var，var会提升到函数作用域顶部，那么会提升到全局，造成变量污染。var重复声名式没有问题的。
			for (let i = 0; i < 3; i++) {
				let i = 'abc'
				console.log(i) //'abc' *3
            }
            console.log(i); //not defined!!
```

###### let和const的块级作用域暂时性死区

```
暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。
```

###### function趣谈

- ```javascript
  function fn(x, y){
      //函数体
  }, 
  fn(),
  fn(1,2)
  //函数内部相当于隐式let声明了x, y，此时如果再用let声名x或y，就会报错，不能重复声名x或y
  //在执行函数体之前，执行对隐式声明的形参的赋值操作，即x=1, y=2
  ```

- ```javascript
  function bar(x = y, y = 2) {
    //函数体
  }
  bar(); // 报错
  //let暂时性死区的一种表现。在函数内部，使用let隐式声明了x, y，但是在声名y之前就使用了y赋值，所以报错。
  ```

- ```javascript
  //函数名括号内的代码，会现在函数内先 编译 执行，然后再执行实参的赋值操作，然后再执行函数体
  //例如这个代码报的是这个错误:Cannot access 'x' before initialization
  function bar(y = x,x = 2) {
      var x =3 //这里的var x 不能提升到()内部之前，()内的代码会先经过预编译阶段
  }
  bar() //Cannot access 'x' before initialization，预编译阶段的错误
  ```


###### script标签

script标签不是作用域的标志，只有{}才是。比如下面

```javascript
<script>
	var a = 1;
	let b = 2;
</script>
<script>
	var a = 3;
	let b = 4;  //报错，b已经声名了
</script>
```





