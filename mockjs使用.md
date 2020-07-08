### mockjs使用

##### 线上环境而不是生产环境中需要使用假的接口，所以要加判断，在main.js中添加下面这句，在生产环境中就mock就不会拦截axios请求：

```javascript
process.env.NODE_ENV != 'production' && require('./mock/mock.js'); //如果是生产环境则不加载
```

##### 新建mock.js文件，生成mock数据。比如

```javascript
//mock.js
var Mock = require('mockjs')
Mock.mock('http://123.207.32.32:8000/api/m3', {    //拦截这个特定url的请求
        'token|5-20': 'mockstring'
})
```



#### 关于mockjs生成数据的用法



