## vue-router使用指南

写在最前：针对于构建化的使用文档

[TOC]

#### 安装

- vue-cli项目可以便捷的安装vue-router,`vue add router`后按照提示按照即可

  ```javascript
  vue add router
  ```

- 普通方式安装

  ```javascript
  yarn add vue-router --save
  // main.js引入文件
  import VueRouter from 'vue-router'
  Vue.use(VueRouter)
  ```

#### 起步

定义路由页面

```javascript
// router.js
import VueRouter from 'vue-router'
import HelloWorld from '../views/helloworld'
const routes = [{
    path:'/hello',
    name:'hello',
    component:HelloWorld
}]
const router = new VueRouter({routes})
export default router
// 改造main.js
import router from './route/router'
const app = new Vue({
    router
}).$mount('#app')
```

通过注入路由器，我们可以在任何组件内通过 `this.$router` 访问路由器，也可以通过 `this.$route` 访问当前路由
***要注意，当 `<router-link>` 对应的路由匹配成功，将自动设置 class 属性值 `.router-link-active`***

#### 动态匹配路由

在定义路由时预留参数，方式很简单，修改routes变量

```javascript
const routes = [{
    path:'/hello/:id',
    name:'hello',
    component:HelloWorld
}]
```

***你可以在一个路由中设置多段“路径参数”，对应的值都会设置到 `$route.params` 中***

|  **模式**  | **匹配路径** | **$route.params** |
| :--------: | :----------: | :---------------: |
| /hello/:id |   /hello/1   |      {id:1}       |

***除了 `$route.params` 外，`$route` 对象还提供了其它有用的信息，例如，`$route.query` (如果 URL 中有查询参数)、`$route.hash` 等等***

##### 响应路由参数的变化

当使用路由参数时，例如从 `/hello/1` 导航到 `/hello/2`，**原来的组件实例会被复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用**。
解决方法：

- 监听路由变化

  ```javascript
  watch: {
      $route(to, from) {
        // 对路由变化作出响应...
      }
  }
  ```

- vue2.2引入的`beforeRouteUpdate` [导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)：

  ```javascript
  beforeRouteUpdate (to, from, next) {
      // 做你的事情
      // 千万别忘记调用next()
  }
  ```


##### 捕获所有路由或 404 Not found 路由

```javascript
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}
```





