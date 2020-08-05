### 浏览器location对象 与vue-router的$router/$route对象 与Ajax的参数



#### 浏览器location对象

https://www.baidu.com/home/index.html?name=zhangsan&&age=18

href: https://www.baidu.com/home/index.html?name=zhangsan&&age=18

protocal: https:

host/hostname :  www.baidu.com

pathname:  /home/index.html

search:  ?name=zhangsan&&age=18

origin:  https://www.baidu.com



#### $route 当前路由

```js
routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User, name: 'USER' }  //这个对象称之为路由对象，或路由记录
  ]
实际路径：
/user/evan?uname=andy&&age=18
```

获取：

$route.path :  /user/evan

$route.params  : { id : 'evan' }  路由参数。

当路由参数params或查询query变化时，组件会复用，不会重新创建，意味着一系列钩子函数不会被执行。可以在组件内使用watch: {$route(to, from){//something}}或者组件内的导航守卫beforeRouteUpdate (to, from, next) {//something and next()}

$route.query : { uname: 'andy', age: 18}

$route.name :   'USER'  当前路由组件的名称，如果有的话。

$route.hash :  #的字符串，String类型

$route.fullPath :  全部的字符串



#### $router 路由器

可以在每个组件中使用this.$router访问这个全局对象

**$router.currentPath 就是 $route**

##### push方法

path和query更搭配，name和params更搭配。path带params时，要在path中写全。

```js
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

```js
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

##### 其他方法：

router.replace()

router.go(n)



#### 导航（路由）守卫

##### 全局前置守卫 router.beforeEach()

导航是异步的，导航在守卫解析完成之前一直处于等待状态。只有当解析完成之后，执行next()函数，才会执行导航，具体导航地址由next()函数的参数决定。不要忘了一定要执行next()函数进行导航。

```javascript
const router = new VueRouter({...})
router.beforeEach((to, from, next)=>{...})

to: Route: 即将要进入的目标 路由对象

from: Route: 当前导航正要离开的路由

next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。

next(false): 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。

next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: 'home' 之类的选项以及任何用在 router-link 的 to prop 或 router.push 中的选项。

next(error): (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。
```

##### 全局解析守卫 router.beforeResolve()

和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

##### 全局后置钩子 router.afterEach()

导航后调用。不会改变导航（因为没有next()）

##### 路由独享守卫 beforeEnter

进入到某一个具体的路由对象前，调用的函数。与全局前置守卫参数相同功能相似

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

##### 组件内守卫

路由对象对应的组件内部，设置的守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
    next(vm => {
    // 通过 `vm` 访问组件实例
  	})
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
    this.name = to.params.name
  	next()
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

##### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。



#### 路由元信息

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

例如，根据上面的路由配置，`/foo/bar` 这个 URL 将会匹配父路由记录以及子路由记录。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 `meta` 字段。

下面例子展示在全局导航守卫中检查元字段：

```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```