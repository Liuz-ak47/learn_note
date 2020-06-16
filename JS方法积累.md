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





