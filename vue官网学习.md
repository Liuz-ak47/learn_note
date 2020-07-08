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
        
      双向绑定不一定是输入框，所以重点是父组件的data，传到自组件的prop中，再被动态属性绑定，决定显示
      
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