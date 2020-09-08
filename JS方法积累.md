### JS方法积累

#### 对象

##### 遍历对象   

```javascript
*****************注意对象没有for（let i of obj）{}方法 和 obj.forEach()方法****************
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
4.
Object.entries() 方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。
例如：
(1)
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]
(2)
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.entries(anObj)); // [ ['2', 'b'], ['7', 'c'], ['100', 'a'] ]
(3)
const object1 = {a: 'somestring', b: 42};
for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}
// "a: somestring"
// "b: 42"

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
3.every 
every() 方法使用指定函数检测数组中的所有元素：
如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。
如果所有元素都满足条件，则返回 true。
例如：
var ages = [32, 33, 16, 40];
function checkAdult(age) {
    return age >= 18;
}
function myFunction() {
    document.getElementById("demo").innerHTML = ages.every(checkAdult);
}

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

增删改：
let arr = ['word', 'dd', 'ddds']; 
arr.splice(0,1,'aa'); //['word']
arr // ['aa', 'dd', 'ddds']


```



#### 字符串

```
let str = 'abcd'

截取：
str.slice(1) //'bcd' 不改变原数组，返回截取
```





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



















