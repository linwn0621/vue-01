## day2

### 目标

> vue中的全局组件组件
>
> 什么是单文件组件
>
> 自己来配置Webpack支持单文件组件
>
> 重温脚手架-手动设置
>
> vue基础-基于脚手架
>
> 一堆指令
>
> 案例



### 我的第一个vue程序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- 1.引入vue.js，当引入vue.js之后，就会在当前系统对象上挂载一个Vue构造函数，通过它可以创建vue对象 -->
    <script src='./js/vue.js'></script>
</head>
<body>
    <!-- vue的相关功能只有在模板中才有效,脱离了模板则会原样输出 -->
    <!-- <p>我是{{myname}},我今年{{age}}岁</p> -->
    <!-- 4. 定义模板:以后vue的相关的数据和功能只能在这个结构中进行解析,离开这个结构则失效 -->
    <div id="app">
        <!-- 使用"插值表达式"展示数据 == 输出表达式 -->
        <p>我是{{myname}},我今年{{age}}岁</p>
        <p>这是我的第一个vue程序</p>
    </div>
    <script>
        // 2.创建vue实例:它可以实现模板和数据的关联
        // 意味着你得告诉他模板是那个,数据有哪些,所以我们需要在vue实例中进行相关的配置
        var vm = new Vue({
            // 3.进行配置
            // 3.1 配置模板,通过el来指定模板结构
            el:'#app',
            // 3.2 配置数据,通过data定义数据,它是一个对象
            data:{
                myname:'jack',
                age:20
            }
        }) 
        // mvvm模式:vm实例可以实现数据和模板关联,以后我们的精力可以着重放在数据的处理上,在vue中,只要数据有变化,模板内容就会有相应的变化.
        // m:model,数据
        // v:view,模板
        // vm:vue实例,实现数据的动态渲染,将内容渲染到页面中
    </script>

</body>
</html>
```

mvvm模式:vm实例可以实现数据和模板关联,以后我们的精力可以着重放在数据的处理上,在vue中,只要数据有变化,模板内容就会有相应的变化.

​        m:model,数据

​        v:view,模板

​        vm:vue实例

### vue中的全局组件组件

> 组件--标签，vue中的组件可以简单理解为：拥有相应结构，功能和样式的标签，我们可以定义单文件组件，当成标签来使用
>
> 演示的环境：引入vue.js文件

#### 创建和使用的步骤

> 引入vue.js
>
> 创建vm实例
>
> 创建全局组件
>
> 使用全局组件

##### 创建全局组件

语法: var 组件对象 = Vue.component(‘组件名称’，{配置})

组件名称：类似于标签名称，后期通过这个名称来使用这个组件

组件是可复用的 Vue 实例

1.可复用，说明创建好的组件可以重复使用

2.它是一个Vue实例，因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项

> 组件的配置成员

1.template:指定当前组件的模板结构

```tex
template的重大细节

	a.错误信息：Component template should contain exactly one root element. If you are using v-if on multiple elements, use v-else-if to chain them instead.：组件只能包含一个根元素
	b.以后创建组件的时候，先添加根元素，再在根元素添加其它的元素
```

2.data:可以配置组件所关联的数据

```tex
组件中的data必须是一函数

错误信息1：The "data" option should be a function that returns a per-instance value in component definitions：data选项必须是一个函数，这个在组件定义的时候可以让每个组件返回一个单独的实例
错误信息2：Property or method "****" is not defined on the instance but referenced during render：属性或方法****在组件实例中没有定义，但是你组件渲染时你引用了，**所以数据和方法一定要先定义再使用**
错误信息3：data functions should return an object：data函数一定要返回一个对象，如果你添加了这个选项的话

a.因为组件可以被复用，如果Data直接是一个对象，那么就会造成在复用时所有组件指向同一个对象，修改一个组件中的数据，其它的组件数据也会被修改
b.如果使用函数返回一个对象，那么每个组件就有一个属于他自己的数据对象
```



##### 使用全局组件

1.像使用标签一样来使用全局组件

2.只有在vm实例所对应的模板结构中才能使用



##### 代码的两个小细节：

1.关于组件命名

```tex
错误信息：Unknown custom element: <myfirst> - did you register the component correctly? For recursive components, make sure to provide the "name" option。这个错误告诉我们，你是否正确的创建和注册了这个组件，说白了，就是它不认为你使用到的组件正确的定义了。如果你定义组件的时候，名称使用了cemal命名法，那么在使用的时候需要使用-进行每个单词的连接，并且单词尽量都使用小写，如：Vue.component('myCom'),那么使用的时候应该：<my-com>或者<my-Com>,所以建议，以后组件的命名都使用小写
```

2.关于代码顺序：先定义组件再创建vm实例



### 单文件组件

> 它解决了全局组件的缺点，并且将样式提升到一个前所未有的高度,并且支持webpack打包构建
>
> 一个单文件组件就相当于一个标签元素，它包含三个部分：结构，功能，样式
>
> 结构：template：只有一个根元素
>
> 功能：script：返回默认对象
>
> 样式：style：可以使用预处理器

#### 创建单文件组件

```vu
<template>
    <!-- 定义组件结构 -->
    <div class="first">
        <h1>我的姓名叫:{{myname}}</h1>
    </div>
</template>


<script>
// 由于单文件组件相当于一个封装的标签元素,并且可以复用,意味着它将来应该需要被引入来使用
// 所以,我们需要将这个组件对象暴露
// 这个对象就是当前的组件对象,当单文件组件编译成功之后,会将对应的结构,样式,功能,进行整合,生成一个对象并暴露
// 虽然是分为三个部分写的,但是它们是一个整体
// module.exports = {} // node写法
// 下面这种写法才是建议的写法
// 导出一个默认对象,这个对象就是当前的组件对象,由于是默认对象,意味着以后引入这个单文件组件就会默认返回当前的组件对象
export default {
    // 组件的Data是一个函数
    data(){
        return {
            myname:'jack'
        }
    }
}
</script>
<style>
    h1{
        color:red;
    }
</style>
```



#### 使用单文件组件

> 就是想让我们所创建的单文件组件的内容在页面中出现

![1573282640073](images\01-单文件组件的渲染.png)



上面这个图片告诉我们，我们应该在main.js中写代码

1.引入你想渲染的单文件组件

2.下载安装vue

3.引入vue

4.创建vue实例，实现渲染功能

```js
// 1.引入单文件组件
import First from './components/First.vue'
// 2.引入Vue
import Vue from 'vue'

// 3.渲染
new Vue({
    el:'#app',
    // 在vue实例中有一个内置成员:render
    // 这个函数有一个参数,参数才是真正的渲染函数
    // 下面这个fn函数可以渲染组件,并将组件内容返回,在指定的模板结构中展示
    render:(fn) => {
        return fn(First)
    }
})
```

完成上面的任务之后，以现报错了：webpack还不支持vue这种类型的文件

1.百度搜索应该添加什么样的loader：vue-loader

2.下载包:npm install -D vue-loader vue-template-compiler

3.配置

```js
// 添加vue文件的支持
{
    test: /\.vue$/,
	loader: 'vue-loader'
}
const VueLoaderPlugin = require('vue-loader/lib/plugin')
new VueLoaderPlugin()
```



### 脚手架

1.创建项目 vue create 项目名称

2.创建项目的过程

![1573285536282](images\02-mydemo1.png)

![1573285648108](images\03-mydemo2.png)

![1573285692019](images\04-mydemo3.png)

![1573285761300](\images\05-mydemo4.png)

![1573285826859](images\06-mydemo5.png)

![1573285881535](images\07-mydemo6.png)

![1573285935162](images\08-mydemo7.png)



### 常用指令

#### 插值表达式：{{}}

1.作用：作用就是用来展示数据的,这个数据变量一定要先定义好

2.基本语法 ：{{变量}}

3.特性：

- 变量一定要先定义好，否则报错
- 里面可以进行基本的拼接
- 还可以实现基本的运算
- 还可以调用api
- 还可以使用三元表达式

4.细节

- 可以写表达式
- 不能写语句
- 不能当成属性来使用

```vue
<template>
    <div class="first">
        <h1>这个文件用来说明插值表达式的使用</h1>
        <h2>作用就是用来展示数据的,这个数据变量一定要先定义好</h2>
        <p>{{msg}}</p>
        <p>我想对你说：{{msg}}</p>
        <p>{{'我想对你说：' + msg}}</p>
        <p>{{age + 1}}</p>
        <p>{{msg.toUpperCase()}}</p>
        <p>{{age>=18?"成年":"未成年"}}</p>
        <!-- <p>{{if(age>=19){console.log(123)}}}</p> -->
    </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'hello world',
      age: 20
    }
  }
}
</script>
```



#### v-text

1.作用：可以设置标签之间的数据展示

2.基本语法 ：

- 它的使用方式和标签的属性一样
- <标签 v-text='变量'></标签>

3.特性

- 它是完全的替换标签之间的内容
- 其它特性和插值一样

4.不能写在标签之间，它只能做为属性来使用

```vue
<template>
  <div class="text">
      <h1>这个文件说明v-text的使用</h1>
      <p>{{msg}}我还有其它内容哦</p>
      <p v-text='msg+"Aa".toUpperCase()'>我本来还有其它内容哦</p>
      <!-- 写在标签之间就被认为是一个普通的字符串了吧 -->
      <p>v-text='msg'</p>
      <!-- <p v-text="{{msg}}"></p> -->
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: '你好啊'
    }
  }
}
</script>
```



#### v-html

1.作用：可以解析html结构，而不是当成普通字符串处理

2.基本语法 

- 它是一个属性的使用方式:<标签 v-html='变量'></标签>

3.特性

- 如果碰到html标签结构，则会进行解析处理
- 其它特性与{{}}和v-text一样

```vue
<template>
  <div class="html">
      <h1>这个文件说明v-html的使用</h1>
      <p>{{msg}}</p>
      <p v-text='msg'></p>
      <p v-html="msg.toUpperCase()+'<hr>'"></p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'aa你<br/>好啊bb alert(11)'
    }
  }
}
</script>
```



#### v-bind

1.作用：标记当前指定属性的值是一个变化的值，它可以让属性值动态的绑定为变量

2.语法：<标签 v-bind:属性='值'></标签>

​	简写： 使用：实现动态绑定

3.特性：

- 为属性动态绑定数据
- 为任意的属性实现动态绑定

4.常见的三大使用场景

- 为属性动态绑定变量
- 动态绑定样式
- 组件传值

```js
<template>
    <div class="bind">
        <h1>这个文件说明v-bind的使用</h1>
        <!-- <a href="/del?id=id">删除</a> -->
        <a v-bind:href="'/del?id='+id">删除</a>
        <!-- 为动态属性绑定动态值 -->
        <p v-bind:[key]="id">有没有存储好？</p>
        <!-- 缩写 -->
        <p :id="id">有没有id</p>

        <!-- 动态绑定样式 -->
        <button @click='isCollapse=!isCollapse'>展开和合并</button>
        <div :class="{menu:true,collapse:isCollapse}"></div>
        <!-- <div :class="[menu,collapse]"></div> -->
        <p :style='"font-size:"+fontSize'>我是p元素</p>
    </div>
</template>

<script>
export default {
  data () {
    return {
      id: 5,
      key: 'username',
      isCollapse: false,
      menu: 'menu',
      collapse: 'collapse',
      fontSize: '30px'
    }
  }
}
</script>
```



#### v-for

1.作用：实现循环遍历 ，主要用于遍历 数组和对象

2.特性：你想循环生成什么结构，就在这个结构中添加v-for

3.语法

```tex
遍历数组
<标签 v-for='(value,index) in arr' :key='唯一值'></标签>
```

```tex
遍历对象
<标签 v-for='(value,key,index) in obj' :key='唯一值'></标签>
```

4.细节：

- eslint要求我们设置:key,它可以唯一的标识这行记录，在后期进行数据操作的时候，可以提高操作效率
- 如果有多个属性需要使用，则使用()包含，如果只有一个，不需要使用()

```vue
<template>
  <div class="demo">
      <h1>对象数组的遍历</h1>
      <table border='1'>
          <thead>
              <tr>
                  <th>姓名</th>
                  <th>年龄</th>
                  <th>性别</th>
                  <th>爱好</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for='(value,index) in stuList' :key='index'>
                  <!-- <td v-for="(v,k) in value" :key='k'>{{v}}</td> -->
                  <td>{{value.name}}</td>
                  <td>{{value.age}}</td>
                  <td>{{value.gender}}</td>
                  <td>{{value.hobby?value.hobby:''}}</td>
              </tr>
          </tbody>
      </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      stuList: [ // 数组
        { // 对象
          name: 'jack',
          age: 20,
          gender: '男'
        },
        {
          name: 'rose',
          age: 18,
          gender: '女',
          hobby: '写代码'
        }
      ]
    }
  }
}
</script>
```



































