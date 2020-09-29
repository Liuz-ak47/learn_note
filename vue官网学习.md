### vue官网

1. 动态组件

   ```
   通过component标签的is属性，修改组件，cpn-name是一个组件的名称，可以是一个计算属性
   <component v-bind:is="cpn-name"></component>
   动态组件可以使用<keep-alive></keep-alive>标签包裹，达到缓存的目的，即记录状态保持不被销毁。
   ```

2. 隔代通信的方式：依赖注入

   *类似于props，但是props只能在父子组件传递*

   ```
   可以在外层组件使用:
   provide(){
   	return{
   		map: this.map   //方法/数据都行
   	}
   }
   内层组件使用：
   inject: ['map']   //用来接收祖先组建的传过来的数据
   ```


3. Vue.set(target, property/index, value)  

   向响应式对象或数组目标中添加成员，确保可以做到响应式的触发更新视图。

   target: Object | Array

   property/index: string/number

   value: any

4. v-model双向绑定

   1. 标签双向绑定：

      ```javascript
      <input type="text" v-model="data">
          相当于
      <input v-bind:value="data" v-on:input="data=$event.target.value">
      ```

   2. 自定义组件双向绑定：

      *目的：外面父级作用域能够改变组件内部的显示，组件内部更改值能改变父级作用域的数据（通过&emit v-on的方式）*

      ```javascript
      <custom-input v-model="data"></custom-input>
      	相当于下面这样的简写
      <custom-input v-bind:value="data" v-on:input="data=$event"></custom-input>
      	在自定义组件内部应该定义一个名字为value的prop
          //custom-input内部
      <input type="text" v-bind:value="value" v-on:input="$emit('input', $event.target.value)">
      {	
          model: {  //model规定了子组件中双向绑定的方式，事件名称
              prop: "value"
              event: "input"
          }
          props: ['value'],
      }
        
      双向绑定不一定是输入框，所以重点是父组件的data，传到自组件的prop中，由子组件的model决定props接收的名称，比如单选复选框为checked等，并决定改变事件，如change等。
      
      ```

5. 父子组件props的双向绑定

   ```javascript
   通过.sync修饰符   
   实现了组件内部props:['title']属性与外部doc.title的双向绑定
   <text-document v-bind:title.sync="doc.title"></text-document> 
   
   组件内部改变props数据通过this.$emit('update:title', newTitle)方法主动改变数据
   外部是通过监听这个方法，改变数据
   <text-document v-bind:title="doc.title" v-on:update:title="doc.title = $event"></text-document> 
   简写如上
   ```


6. Vuex的gettets和actions的辅助函数

   ### `mapGetters` 辅助函数

   `mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：

   ```js
   import { mapGetters } from 'vuex'
   
   export default {
     // ...
     computed: {
     // 使用对象展开运算符将 getter 混入 computed 对象中
       ...mapGetters([
         'doneTodosCount',   //vuex的store中的getters中的方法
         'anotherGetter',
         // ...
       ])
     }
   }
   ```

   如果你想将一个 getter 属性另取一个名字，使用对象形式：

   ```js
   ...mapGetters({
     // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
     doneCount: 'doneTodosCount'
   })
   ```

7.  如果在vue组件中定义一个属性a，使用vuex中的state属性b来初始它的值，那么后续这两个值是互不影响的，即修改a state中的b不会变，mutations修改b，组件中a的值也不会变，不管a、b是简单数据类型还是复杂数据类型。

   ```javascript
   //vue组件
   {
   	data(){
   		return{
   			a: this.$store.state.b
   		}
   	}
   }
   ```

8. 关于父子组件传值，在子组件直接修改props的问题：

   单向数据流的思想，数据只应该从父组件传到子组件，这应该是子组件props数据的唯一来源，每当父组件数据更新时，子组件的props会自动更新到最新。所以在子组件中，可以直接展示、应用props，但是应该避免直接修改props。当需要修改props中的数据时，可以在子组件的data中定义变量并用props数据初始化它，这样相当于用props数据把data数据初始化了，后续props和data数据是相互不影响的（这个的根本原因是因为vue中响应式是把data中数据的属性进行get和set跟踪，而props或任何其他非直接数据再给data数据初始化后，就和data数据脱离了关系）。注意computed对props数据进行转换时，当props数据改变，computed能够输出转换后的值，因为这就是computed的依赖嘛。如果想修改子组件props数据，应该使用vue推荐给我们的思想和方式，即.syc的方式，这种方式的本意是更改父组件的值，让这个值来改变子组件的props值，即维持了单向数据流的思想。

   其实父子组件传值，本质上就是将父组件的数据 复制给子组件的props，所以当直接修改子组件props时，如果传过来的props直接是简单数据，那么不会影响父组件；如果传过来的是对象这种引用数据类型，那么相当于把父组件数据的地址复制给了子组件props，此时修改props任何东西，都会改变父组件的值。

   

   ~~单项数据流的思想，数据只应该从父组件传到子组件，这应该是子组件props数据的唯一来源，每当父组件数据更新时，子组件的props会自动更新到最新。所以在子组件中，可以直接展示、应用props，但是应该避免直接修改props。当需要修改props中的数据类型时，可以在子组件的data或computed中定义变量并用props数据初始化它，由于js对于简单类型数据是变量值复制，所以再修改数据时不会影响props的数据(反过来同样，组件created后，prop简单数据变化后，不再影响子组件数据值)，也就不会影响父组件的原始数据，造成数据改变来源不明确的问题。如果需要修改子组件props中的复杂数据类型，而又不希望影响父组件中的原始数据，那么无论如何也是办不到的。如果想修改子组件props中的复杂数据类型，父组件中原始数据也需要更改，应该使用vue推荐给我们的思想和方式，即.syc的方式。~~

#### 

9. 混入mixins

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





10. .sync 修饰符

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

​	其实父组件是缩写：

```
<exap-father :title="doc.title" @update:title="doc.title = $event"></exap-father>
```

​	当父组件要传递到子组件的是一个含有很多属性的对象，我们可以这么写，已实现这个对象的父子同步修改：

​	//对吗？还没试验

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

​	联想一下，父子组件传值，父组件传递一个对象的全部属性也差不多：

```
<exap-father v-bind="doc"> 
```

11. 
12. 