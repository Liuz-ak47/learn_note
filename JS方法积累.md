### JS方法积累

#### 对象

##### 遍历对象   

```
*****************注意对象没有for（let i of obj）{}方法  和 obj.forEach()方法****************
const obj = {
	a: 'a',
	b: 2,
	c: true
}
1. 
Object.keys(obj)   //['a', 'b', 'c']  得到属性数组
2.
Object.values(obj)  //['a', 2, true]  得到值数组
3.
for(var key in obj){
	console.log(key)  //'a', 'b', 'c'
	console.log(obj[key]) //'a', 2, true
}
```

#### 数组

```
const arr = [1, 2, 3, 4]
1.forEach
let sum = 0;
arr.forEach(t => {
	sum += t;
})
2.map  有返回值
let newarr = arr.map(t => t*2)  //[2, 4, 6, 8]

遍历数组：
1.
for(var i of arr){
	console.log(i)   //1,2,3,4 值
}
for(var i in arr){
	console.log(i)   //0,1,2,3  属性
}

筛选：
let newarr = arr.filter(t => t>2) // [3, 4]

连接：
arr.concat(5, 6)  // [1, 2, 3, 4, 5, 6,] 不会改变原数组
arr.concat(arr1, arr2) // 参数也可以是多个数组

截取slice：
arr.slice(1, 3) // [2, 3]  参数左闭右开，不改变原数组
```



#### 字符串



#### AsyncFunction  构造函数

AsyncFunction 构造函数用来创建新的异步函数对象，JavaScript 中每个异步函数都是 `AsyncFunction` 的对象。

AsyncFunction 并不是一个全局对象，需要通过下面的方法来获取：

```javascript
Object.getPrototypeOf(async function(){}).constructor
```

例子：

通过 AsyncFunction 构造器创建一个异步函数

```js
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

var AsyncFunction = Object.getPrototypeOf(async function(){}).constructor;
var a = new AsyncFunction('a', 
                          'b',
                          'return await resolveAfter2Seconds(a) + await resolveAfter2Seconds(b);');
a(10, 20).then(v => {
  console.log(v); // 4 秒后打印 30
});
```



#### async function异步函数表达式

**`async function`** 关键字用来定义异步函数。可以定义匿名异步函数 (函数体内是显示的异步函数，或者将函数隐式的包装成Promise对象)。

```javascript
async function [name]([arg1[, arg2[, arg3]...]]){函数体}
```

例子：

```javascript
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
};

// async function expression assigned to a variable
var add1 = async function(x) {
  var a = await resolveAfter2Seconds(20);
  var b = await resolveAfter2Seconds(30);
  return x + a + b;
}

add1(10).then(v => {   //注意.then()这样才能渠道return的值，因为async关键字返回的是Promise对象
  console.log(v);  // 4 秒后打印 60
});

```

#### await  

作用1：等待当前Promise对象执行完毕

await 操作符用于等待一个Promise 对象，等待当前 Promise 处理完成，再释线程放执行权。它只能在异步函数 async function中使用。

作用2：将Promise的返回值作为表达式的值

await 表达式会暂停当前async function的执行，等待当前 Promise 处理完成，再释线程放执行权。若 Promise 正常处理，**其回调的resolve函数参数作为 await 表达式的值**，继续执行 async function

//即 使多个promise对象顺序执行，后面的要等待前面的resolve结果，再开始执行

例子：

```js
如果一个 Promise 被传递给一个 await 操作符，await 将等待 Promise 正常处理完成并返回其处理结果。
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

async function f1() { 
  var x = await resolveAfter2Seconds(10);  //将Promise的返回值作为表达式的值，赋值给x
  console.log(x); // 10
}
f1();
```

#### 立即执行函数

几种写法：

```javascript
1. 
!function(){

}()
2.
(function(){

}())
3.
(function(){
})()
4.
var main = function(){
	return e.m = n, e(), a=n*n, a=n;
}()

return的语句若为逗号，那么这句话都会执行，并以最后那句a=n作为返回值
```













#### vue官网

##### 混入mixins

数据冲突：组件优先；

钩子函数冲突： 合并为数组，都会执行

值为对象的选项冲突，例如 `methods`、`components` 和 `directives`：对象合并，对象中键名冲突，组件优先



```
var mixin = { 	//定义混入对象
	data: function() {
		reurn{
			a: 'a'
		}
	},
	created: function(){
		console.log()
	},
	methods: {
		fn(){
		
		}
	}
}   

new Vue({
	mixins: [mixin]     //使用混入对象。   
})
```





##### .sync 修饰符

作用:  父子组件传值，当子组件修改值，父组件的属性值同步修改

如：

```
// 父组件
<exap-father :title.sync="doc.title"></exap-father>
//子组件
<exap-son>
	props: {
		title: ''
	},
	methods: {
		sonMethods: function(){
			this.$emit('update:title', newTile)
		}
	}
</exap-son>
```

其实父组件是缩写：

```
<exap-father :title="doc.title" @update:title="doc.title = $event"></exap-father>
```

当父组件要传递到子组件的是一个含有很多属性的对象，我们可以这么写，已实现这个对象的父子同步修改：

//对吗？还没试验

```
//父组件
<exap-father v-bind.sync="doc">  
	data(){
		return{
			doc: {   //doc是父组件的一个对象
				prop1: 'v1',
				prop2: 'v2'
			}
		}
	}
<exap-father>
//子组件
<exap-son >
	props: {
		doc: {}
		//也可能是! 可能性大一些
		// prop1: '',
		// prop2: ''
	},
	methods: {
		sonMethods1: function(){
			this.$emit('update:doc.prop1', newV1)   //this.$emit('update:prop1', newV1)
			this.$emit('update:doc.prop2', newV2)   //this.$emit('update:prop2', newV2)
		}  
	}
</exap-son>
```

联想一下，父子组件传值，父组件传递一个对象的全部属性也差不多：

```
<exap-father v-bind="doc"> 
```

