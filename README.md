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



## Class组件详解

### 两种方式创建Class组件

**ES5方式(过时)**

```
import React from 'react'

const A = React.createClass({
   render() {
      return (
          <div>hi</idv>
      )
   }
})

export default A
```

**ES6方式**

```
import React from 'react';

class B extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
       return (
          <div>hi</div>
       )
    }
}
export default B;
```

### Props-外部数据

**Props的作用**

* 接受外部数据
只能读不能写；</br>
外部数据由父组件传递；

* 接受外部函数
在恰当的时机，调用该函数；</br>
该函数一般是父组件的函数；

### State & setState-内部数据

```
初始化State

class B extends React.Component{
   constructor(props) {
      super(props);
      this.state = {
         users: {name:'wanti',age:18}
      }
    }
    render(){/*...*/}
}
```

**读写State**

* 读用this.state 
`this.state.xxx.yyy.zzz`

* 写用this.setState(???,fn)

this.setState(newState,fn);</br>
注意setState不会立刻改变this.state,会在当前代码运行完后，再去更新this.state,从而触发UI更新；</br>

this.setState((state,props)=>newState,fn);</br>
这种方式的state反而更易于理解；</br>

fn会在写入成功后执行；</br>


### 生命周期

**类比如下代码**

```
let div = document.createElement('div')
//这是div的create/construct过程

div.textContent = 'hi'
//这是初始化state

document.body.appendChild(div)
//这是div的mount过程

div.textContent = 'hi2'
//这是div的update过程

div.remove()
//这是div的unmount过程
```

React组件中也有这些过程，我们称之为生命周期

### 生命周期

**函数列表**

* constructor()
* static getDerivedStateFromProps()
* shouldComponentUpdate()
* render()
* getSnapshotBeforeUpdate()
* componentDidMount()
* componentDidUpdate()
* componentWillUnmount()
* static getDerivedStateFromError()
* componentDidCatch

**生命周期-必会**

* 函数列表

constructor()-在这里初始化state；</br>
shouldComponentUpdate()-return false组织更新;</br>
render()-创建虚拟DOM;</br>
componentDidMount()-组件已出现在页面;</br>
componentDidUpdate()-组件已更新;</br>
componentWillUnmount()-组件将死;</br>

#### constructor

**用途**

初始化props;</br>
初始化state,但此时不能调用setState;</br>
用来写bind this </br>
```
constructor() {
   ...
   this.onClick = this.onClick.bind(this)
}

使用新语法代替
onClick = () => {}
constructor(){ ... }
```

#### shouldComponentUpdate

**用途**

返回true表示不阻止UI更新；</br>
返回false表示阻止UI更新；</br>

**作用**

它允许我们手动判断是否要进行组件更新，我们可以根据应用场景灵活地设置返回值，以避免不必要的更新

#### render

**用途**

展示视图：
   `return(<div>...</div>)`</br>
只能有一个根元素；</br>
如果有两个根元素，就要用<React.Fragment>;</br>
<React.Fragment />可以缩写为<></></br>

**技巧**

render里面可以写if...else;</br>
render里面可以写?:表达式；</br>
render里面不能直接写for循环，需要使用数组；</br>
render里面可以写array.map(循环)；</br>

#### componentDidMount()

**用途:**

在元素插入页面后执行代码，这些代码依赖DON；</br>
例如想获取div的高度，就可以在这里写；</br>
此处可以发起加载数据的AJAX请求；</br>
首次渲染会执行此钩子；</br>

#### componentDidUpdate()

**用途：**

在视图更新后执行代码；</br>
此处也可以发起AJAX请求，用于更新数据；</br>
首次渲染不会执行此钩子；</br>
在此处setState可能会引起无限循环，除非放在if里；</br>
若shouldComponentUpdate返回false,则不出发此钩子；</br>

#### componentWillUnmount

**用途**

组件将要被移出页面然后被销毁时执行代码；</br>
unmount过的组件不会再次mount;

**举例**

如果你在c..DidMount里面监听了window scroll;
那么你就要在c...WillUnmount里面取消了监听

如果你在c...DidMount里面创建了Timer;
那么你就要在c...WillUnmount里面取消了timer;

谁污染谁治理；

#### 生命周期回顾

**函数列表**

* constructor() - 在这里初始化state
* shouldComponentUpdate() - return false组织更新
* render() - 创建虚拟DOM
* componentDidMount() - 组件已出现在页面
* componentDidUpdate() - 组件已更新
* componentWillUnmount() - 组件将死

*不要忘了renturn true*


####

## React函数组件

### 创建方式

**函数组件**
```
const Hello = (props) => {
    return <div>{props.message}</div>
}

const Hello = props => <div>{props.message}</div>

function Hello(props){
    return <div>{props.message}</div>
}
```

**函数组件代替class组件**

* 面临两个问题：

函数组件没有state;</br>
函数组件没有生命周期；</br>

* 没有State
React v16.8.0推出Hooks API,其中一个API叫做useState可以解决问题。

* 没有生命周期
React v16.8.0推出Hooks API,其中一个API叫做useEffect可以解决问题。

### useEffect

```
模拟componentDidMount
useEffect(()->{ console.log('第一次渲染') },[])

模拟componentDidUpdate
useEffect(()=> { console.log('任意属性变更')});
useEffect(()=>{ console.log('n变了')},[]);

模拟componentWillUnmount
useEffect(()=>{
   console.log('第一次渲染')
   return ()=>{
      console.log('组件要死了')
   }
})
```



