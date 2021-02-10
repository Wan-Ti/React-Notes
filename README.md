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



## React组件

### 组件

#### Element V.S. Component

**元素与组件**

`const div = React.createElement('div',...)`</br>
这是一个React元素(d小写)

`const Div = ()=>React.createElement('div'..)`</br>
这是一个React组件(D大写)

**什么是组件**

能与其他物件组合起来的物件就是组件；</br>
就目前而言，一个返回React元素的函数就是组件；</br>
在Vue里，一个构造选项就可以表示一个组件；</br>

#### React两种组件

**一、函数组件**

```
function Weclome(props){
   return <h1>Hello,{props.name}</h1>;
}
使用方法： <Welcome name="wanti" />
```

**二、类组件**

```
class Welcome extends React.Component {
  render() {
      return <h1>Hello,{this.props.name}</h1>
   }
}
使用方法：<Weclcom name="wanti"/>
```

**<Welcome />**

* React.createElement的逻辑
如果传入一个字符串'div',则会创建一个div;</br>
如果传入一个函数，则会调用该函数，获取其返回值；</br>
如果传入一个类，则在类前面加个new(这会导致执行constructor),获取一个组件对象，然后调用对象的render方法，获取其返回值；</br>

**类组件注意事项**

* this.state.n += 1无效？
其实n已经改变了，只是UI不会自动更新而已；</br>
调用setState才会触发UI更新(异步更新)；</br>
因为React没有像Vue监听data一样监听state;</br>

* setState会异步更新UI
setstate之后，state不会马上改变，立马读state会失败；</br>
更推荐的方式是setState(函数)；</br>

#### 两种编程模型

**Vue的编程模型**

一个对象，对应一个虚拟DOM。当对象的属性改变时，把属性相关的DOM节点全部更新。

注：Vue为了其他考量，也引入了虚拟DOM和DOM Diff.

**React的编程模型**

一个对象，对应一个虚拟DOM；另一个对象，对应另一个虚拟DOM。对比两个虚拟DOM，找不同(DON diff)最后局部更新DOM。

### 事件绑定

```
class Son extends React.Component{
   addN = () => this.setState({n: this.state.n+1});//写法一
   this.addN = ()=> this.setState
   ({n :this.state.n+1})；//写法二
}

//写法一和写法二完全等价

```
该类写法中的函数是对象本身的属性，这意味这每个Son组件都有自己的addN,如果有两个Son,就有两个addN

```
class Son extends React.Component{
   addN(){    
        this.setState({n:this.state.n + 1})
        }//写法一
   addN: function(){
        this.setState({n:this.state.n+1})
        }、、写法二
}
   
 写法一和写法二完全等价
 
```
该类写法中的函数是对象的共用属性(也就是原型上的属性)，也就是说Son组件共用一个addN






