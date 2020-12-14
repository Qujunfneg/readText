####  只解释比较少用的知识点

1. 计算属性

   **计算属性是基于它们的响应式依赖进行缓存的**，只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。相比之下，每当触发重新渲染时，调用方法将**总会**再次执行函数

2. 计算属性的setter

   ```javascript
   computed:{
       fullName:{
           get:function(){
               return this.firstName + ' ' + this.lastName
           },
           set:function(newValue){
              this.firstName = newValue
           }
       }
   }
   ```

#### 组件注册

1. `vue`全局组件注册后报错

   `vue-cli3.0`解决办法:

   ```javascript
   module.exports = {
     runtimeCompiler: true,
   }
   ```

2. ```javascript
   Vue.component('my-com',Vue.extend({
     template:'<div>123</div>'
   }))
   ```

3. 组件名的大小写

   #### 使用 kebab-case

   ```javascript
   Vue.component('my-component-name', { /* ... */ })
   <my-component-name>
   ```

   #### 使用 `PascalCase`

   ```javascript
   Vue.component('MyComponentName', { /* ... */ })
   <my-component-name> 和 <MyComponentName>
   ```

#### 自定义事件

不同于组件和 prop，事件名不存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称。

- 自定义组件的v-model

  ```javascript
  <base-input v-model='aaa'></base-input>
  // data--> aa=''
  Vue.component('base-input',{
      props:['proval'],
      model:{
          props:'proval',
          event:'change'
      },
      template:`
  		<input type='text' @change='$emit("change",$event.target.value)' :value='proval'>
  	`
  })
  ```

- .sync 修饰符

  ```javascript
  <text-document v-bind:title.sync="doc.title"></text-document>
  //props接受title
  this.$emit('update:title', newTitle)
  // 重点 update
  ```

#### 插槽

- 无名插槽

```javascript
<navigation-link url="/profile">
  Your Profile
</navigation-link>
// navigation-link组件中
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

- 具名插槽

  ```javascript
  # slot的特殊属性name
  <slot name='header'></slot>
  // 使用
  <navigation-link url="/profile">
    <template v-slot:header>
        <a>123213</a>
    </template>
  </navigation-link>
  ```

- 作用域插槽

  ```javascript
  <span>
    <slot v-bind:user="user">
      {{ user.lastName }}
    </slot>
  </span>
  ###########################
  <current-user>
    <template v-slot:default="slotProps">
      {{ slotProps.user.firstName }}
    </template>
  </current-user>
  ```

  只要出现多个插槽，请始终为*所有的*插槽使用完整的基于 `<template>` 的语法：

  ```javascript
  <current-user>
    <template v-slot:default="slotProps">
      {{ slotProps.user.firstName }}
    </template>
  
    <template v-slot:other="otherSlotProps">
      ...
    </template>
  </current-user>
  ```

  