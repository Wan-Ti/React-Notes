# React

## 如何引入React

**从CDN引入**

引入React: http://.../react.x,min.js;</br>
引入ReactDOM: http://.../react-dom.x.min.js

**cjs和umd的区别：**

cjs全称CommonJS，是Node.js支持的模块规范；</br>
umd是统一模块定义，兼容各种模块规范；</br>
理论上优先使用umd,同时支持Node.js和浏览器；</br>
最新的模块规范是使用import和export关键字；</br>

**通过webpack引入React**

* import...from...
```
yarn add react react-dom;
import React from 'react';
import ReactDOM from 'react-dom';
```
注意大小写，尽量保持一致；

### 对比React元素和函数组件

**对比：**
```
App1 = React.createElement('div',null,n)
App1是一个React元素

App2 = ()=> React.createElement('div',null.n)
App2是一个React函数组件
```

函数App2是延迟执行的代码，会在被调用的时候执行

**React元素**

createElement的返回值element可以代表一个div,但element并不是真正的div(DOM对象)

**() => React元素**

返回element的函数，也可以代表一个div;</br>
该函数可以多次执行，每次得到最新的虚拟div,React会对比两个虚拟div,找出不同，局部更新视图。</br>
找不同的算法叫做DOM Diff算法

### 使用JSX

**Vue有vue-loader**

.vue文件里写`<template><script><style>`;</br>
通过vue-loader变成一个构造选项；</br>

**React有JSX**

例如：将`<button onClick="add">+1<?button>`变成`React.createElement('button',{onClick:...},'+1')`

**方法一：CDN**

引入babel.min.js;</br>
把`<script>`改为`<script type="text/babel">`;</br>
babel会自动进行转义；</br>

**方法二：webpack + babel-loader**

**方法三：使用create-react-app**

* 跟@vue/cli用法类似；
* 全局安装yarn global add create-react-app
* 初始化目录create-react-app react-demo-1
* 进入目录cd react-demo-1
* 开始开发yarn start
* 看看App.js，是否默认使用了jsx语法

**使用JSX的注意事件**

* 注意className
```
<div className="red">n</div>
被转译为
React.createElement('div',{className:'red'},"n")
```

* 插入变量

标签里面的所有JS代码都要用{}包起来；</br>
如果需要变量n,那么就用{}把n包起来；</br>
如果需要对象，那么就用{}把对象包起来，如{{name:'frank'}};</br>

* 习惯return后面加()

***Vue将HTML写在.vue文件的`<template>`里***

***React把HTML混在JS / JSX文件里***


### if...else...

条件控制语句

**条件判断**

```
在Vue里

<template>
    <div> 
        <div v-if="n%2===0">
          n是偶数      
        </div>
    </div>
</template>
  
```

```
JSX的条件判断

在React里

const Component = () => {
     return n%2===0 ? 
     <div> n 是偶数</div> : <span>n是奇数</span>
// 如果需要外面的div,可以写成
cosnt Component = () => {
    return (
       <div>
           { n%2===0 ? <div>n是偶数</div> : <span>n 是奇数</span> }
        </div>
    )
}

还可以写成

const Component = () => {
    const content = (
        <div>
            { n%2===0 > <div>n是偶数</div> : <span>n是奇数</span>
         </div>
     )
  return content
}

还可以写成


const Component = () => {
   const inner = n%2===0 > <div>n是偶数</div> : <span>n是奇数</span>
   const content = (
      <div> 
          { inner }
      </div>
    )
    return content
}

还可以写成

const Component = () => {
   let inner 
   if (n%2===0){
      inner = <div>n是偶数</div>
   } else {
      inner = <span> n是奇数</span>
   }
   const content = {
        <div>
            { inner }
        </div>
   )
   return content
}
```

### 循环语句

```
在Vue里可以遍历数组和对象

<template>
    <div>
       <div v-for="(n,index) in numbers" :key="index">
       下标 {{index}}，值为{{ n }}
       </div>
    </div>
</template>
```

```
在React里

const Component = (props) => {
   return props.numbers.map((n,index)=>{
     return <div>下标{index}值为{n}</div>
   })
}
//如果你需要外面的div,可以写成
const Component = (props) => {
    return (
    <div>
       { props.numbers.map((n,index)=>{
        return <div>下标{index}值为{n}</div>
        }) }
    </div>)
}


还可以写成

const Component = (props) => {
    const array = []
    for(let i=0;i<props.numbers.length;i++){
      array.push(
      <div>
      下标[i]值为{props.numbers[i]}
      </div>)
   }
return <div>{ array }</div>
}
```

### 总结

**引入React & ReactDOM**

CDN方式：react.js、react-dom.js、babel.js;</br>
也可以直接import React from 'react';</br>

**React.createElement**

创建虚拟DOM对象；</br>
函数的作用：多次创建虚拟DOM对象；</br>
DOM Diff是什么；</br>

**JSX**

将XML转译为React.createElement;</br>
使用{}插入JS代码；</br>
create-react-app默认将JS当作JSX处理；</br>
条件判断、循环要用原生JS实现</br>








