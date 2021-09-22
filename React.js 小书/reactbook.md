# React.js 小书 - 笔记

[React.js 小书在线阅读](https://www.bookstack.cn/read/react-naive-book/about.md)

看的过程中的总结，有一些内容的拓展

## React.js


### 基础知识

MVVM（Model-View-ViewModel）：是 MVC（Model View Controller） 的改进版，一种将图形用户界面开发与业务逻辑或后端逻辑的开发分离开来的软件架构模式。

- Model层：请求的原始数据
- View层：视图展示，由ViewController来控制
- ViewModel层：负责业务处理和数据转化

可以参考 [什么是MVVM](https://www.w3cschool.cn/ios_functional_reactive_program/ios_functional_reactive_program-vjix288i.html)

React 只是一个库，而不是框架，它只提供 UI 也就是 View 层面的解决方案，需要结合其它的库（如 Redux、React-router ）来协助提供完整的解决方法。


### JSX

一个 DOM 元素包含的信息其实只有三个：标签名，属性，子元素，这些信息既可以用 HTML 也可以用 JS 来描述，但 HTML 写起来比 JS 简便许多。 于是 React.js 把 JS 语法扩展了一下，支持直接在 JS 代码里面编写类似 HTML 标签结构的语法，也就是 JSX。JSX 在编译的时候会变成相应的 JavaScript 对象描述。

```
ReactDOM.render(
 <Header />, 
  document.getElementById('root')
)
//会被编译成：
ReactDOM.render(
  React.createElement(Header, null), 
  document.getElementById('root')
)
```

从 JSX 到页面经历的过程：

![](http://huzidaha.github.io/static/assets/img/posts/44B5EC06-EAEB-4BA2-B3DC-325703E4BA45.png)

不直接从 JSX 直接渲染构造 DOM 结构的原因：

元素不一定会被渲染到浏览器页面上，也可能渲染到 canvas 或者 RN 上


### render

一个组件类必须要实现一个 render 方法，这个 render 方法必须要返回一个 JSX 元素。

必须要用一个外层的 JSX 元素（比如<div>）把所有内容包裹起来。返回并列多个 JSX 元素是不合法的。

```
render () {
  return (
    <div>
      <div>第一个</div>
      <div>第二个</div>
    </div>
  )
}
```

因为 class 是 JavaScript 的关键字，所以 React.js 定义了 className 给元素添加类名。

因为 JSX 元素其实是 JS 对象，一次可以将 JSX 元素赋值给变量，或者作为函数参数、函数参数返回值。


### 事件监听

在 React.js 不需要手动调用浏览器原生的 addEventListener 进行事件监听，React.js 帮我们封装好了一系列的 on 属性，当你需要为某个元素监听某个事件的时候，只需要简单地给它加上 on 就可以了。

但是注意，这些 on 的事件监听只能用在普通的 HTML 的标签上，不能用在组件标签上。


### this 

如果想在事件函数当中使用当前的实例，你需要手动地将实例方法 bind 到 this 上再传入给 React.js，例如：

```
class Title extends Component {
  handleClickOnTitle (word, e) {
    console.log(this, word)
  }
  render () {
    return (
      <h1 onClick={this.handleClickOnTitle.bind(this, 'Hello')}>React 小书</h1>
    )
  }
}
```


### state 与 setState

state 用来存储可变化的状态

当我们调用 setState 方法时，React.js 会更新组件的状态 state ，并且重新调用 render 方法渲染页面。

调用 setState 的时候，React.js 并不会马上修改 state，而是把这个对象放到一个更新队列里面，稍后才会从队列当中把新的状态提取出来合并到 state 当中，然后再触发组件更新。

如果需要在后续操作依赖前一个 setState 的结果，可以给 setState 传入一个函数作为参数，就能利用上一次 SetState 的结果了，例如：

```
  handleClickOnLikeButton () {
    this.setState((prevState) => {
      return { count: 0 }
    })
    this.setState((prevState) => {
      return { count: prevState.count + 1 } // 上一个 setState 的返回是 count 为 0，当前返回 1
    })
    this.setState((prevState) => {
      return { count: prevState.count + 2 } // 上一个 setState 的返回是 count 为 1，当前返回 3
    })
    // 最后的结果是 this.state.count 为 3
  }
```

React.js 内部会把 JS 事件循环中的消息队列的同一个消息中的 setState 都进行合并以后再重新渲染组件，因此并不需要担心多次进行 setState 会带来性能问题。


### props

每个组件都可以接受一个 props 参数，它是一个对象，包含了所有你对这个组件的配置。

组件可以在内部通过 this.props 获取到配置参数，props 一旦传入，就不可以在组件内部修改，但可以通过父组件主动重新渲染的方式（比如调用 setState）来传入新的 props，从而达到更新的效果。

state 是让组件控制自己的状态，props 是让外部对组件自己进行配置。因为状态会带来管理的复杂性，推荐多使用无状态组件。


### 状态提升

将组件之间共享的状态交给组件最近的公共父节点保管，然后通过 props 把状态传递给子组件，这样就可以在组件之间共享数据

![](http://huzidaha.github.io/static/assets/img/posts/C547BD3E-F923-4B1D-96BC-A77966CDFBEF.png)

但如果提升路径发生改变，需要修改的地方就很多，这个问题 React.js 还没有很好地解决，不过可以通过 Redux 来管理状态。


### 生命周期

#### 挂载阶段

```
-> constructor()
-> componentWillMount()
-> render()
// 然后构造 DOM 元素插入页面
-> componentDidMount()
// ...
// 即将从页面中删除
-> componentWillUnmount()
// 从页面中删除
```

- constructor：组件自身状态初始化

- componentWillMount：组件启动动作（例如 Ajax 数据的拉取操作、定时器的启动等）

- componentDidMount：依赖 DOM 的启动（例如动画的启动）

- componentWillUnmount：在组件销毁时清除定时器和数据

#### 更新阶段

- shouldComponentUpdate(nextProps, nextState)：可以通过这个方法控制组件是否重新渲染。可以用在性能优化上。

- componentWillReceiveProps(nextProps)：组件从父组件接收到新的 props 之前调用

- componentWillUpdate()：组件开始重新渲染之前调用

- componentDidUpdate()：组件重新渲染并且把更改变更到真实的 DOM 以后调用


### ref 属性

用来获取已经挂载的元素的 DOM 节点然后调用 DOM API

*原则：能不用 ref 就不用* 特别是要避免用 ref 来做 React.js 本来就可以帮助你做到的页面自动更新的操作和事件监听。


### props.children

所有嵌套在组件中的 JSX 结构都可以在组件内部通过 props.children 获取到，可以在各个地方高度复用。

```
class Layout extends Component {
  render () {
    return (
      <div className='two-cols-layout'>
        <div className='sidebar'>
          {this.props.children[0]}
        </div>
        <div className='main'>
          {this.props.children[1]}
        </div>
      </div>
    )
  }
}
```


### dangerouslySetHTML

因为设置 innerHTML 可能会导致跨站脚本攻击（XSS），在 React.js 当中所有的表达式插入的内容都会被自动转义，动态 HTML 结构无法正常渲染

给 dangerouslySetInnerHTML 传入一个对象，这个对象的 __html 属性值就相当于元素的 innerHTML，这样就可以动态渲染元素的 innerHTML 结构


### style 属性

在 React.js 中你需要把 CSS 属性变成一个对象再传给元素：

`<h1 style={{fontSize: '12px', color: 'red'}}>React.js 小书</h1>`

原来 CSS 属性中带 - 的元素都必须要去掉 - 换成驼峰命名，如 font-size 换成 fontSize，text-align 换成 textAlign

用对象作为 style 方便我们动态设置元素的样式，可以用 props 或者 state 中的数据生成样式对象再传给元素，然后用 setState 就可以修改样式，比如 `setState({color: 'blue'})` 就可以把元素的颜色改成蓝色。


### 第三方库 prop-types

给组件的配置参数加上类型验证

`npm install --save prop-types`

```
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class Comment extends Component {
  static propTypes = {
    /*传入的 comment 类型必须为 object*/
    comment: PropTypes.object 
  }
```


### 高阶组件 Higher-Order Components

高阶组件就是一个函数，传给它一个组件，它返回一个新的组件。

`const NewComponent = higherOrderComponent(OldComponent)`

其实就是设计模式里面的装饰者模式


### context

普通情况下一个父组件的状态需要通过 props 一层层往下传递到子组件才能使用。如果组件树层次很深的话维护起来简直是灾难。

而一个组件只要往自己的 context 里放了某个状态，这个组件之下的所有子组件都直接访问这个状态而不需要通过中间组件的传递

子组件要获取 context 里面的内容必须写 contextTypes 来声明和验证需要获取的状态的类型：

```
class Title extends Component {
  static contextTypes = {
    themeColor: PropTypes.string
  }
```

## Redux

Redux 是一种架构模式（Flux 架构的一种变种）

### store

用 createStore 产生的新定义的数据类型，通过 store.getState 获取共享状态，通过 store.dispatch 修改共享状态，通过 store.subscribe 监听数据数据状态。

#### 共享结构对象

用拓展运算符共享结构对象

```
let newAppState = { // 新建一个 newAppState
  ...appState, // 复制 appState 里面的内容
  title: { // 用一个新的对象覆盖原来的 title 属性
    ...appState.title, // 复制原来 title 对象里面的内容
    text: '《React.js 小书》' // 覆盖 text 属性
  }
}
```

![](https://static.sitestack.cn/projects/react-naive-book/5f38c25fc06a9681e0d3c3db2a1cab7e.png)

#### reducer 函数

这个函数规定是一个纯函数，它接受两个参数，一个是 state，一个是 action

reducer 不允许有副作用，不能在里面操作 DOM，也不能发 Ajax 请求，更不能直接修改 state，它能做的仅仅是初始化和计算新的 state

如果没有传入 state 或者 state 为 null，它就会返回一个初始化的数据；如果有传入 state，就会根据 action 来“修改“数据（其实规定不能修改 state，而是要通过拓展运算符共享结构对象）。如果它不能识别你的 action，它就不会产生新的数据，而是（在 default 内部）把 state 原封不动地返回。

#### 套路

```
// 定一个 reducer
function reducer (state, action) {
  /* 初始化 state 和 switch case */
}
// 生成 store
const store = createStore(reducer)
// 监听数据变化重新渲染页面
store.subscribe(() => renderApp(store.getState()))
// 首次渲染页面
renderApp(store.getState()) 
// 后面可以随意 dispatch 了，页面自动更新
store.dispatch(...)
```

### Redux 的安装使用

在工程目录下使用 npm 安装 Redux 和 React-redux 模块：

`npm install redux react-redux --save`

把 src/ 目录下从本地导入的 connect 改成从第三方 react-redux 模块中导入：

`import { connect } from './react-redux'`

改成：

`import { connect } from 'react-redux'`

最后可以删除 src/react-redux.js


## Smart 组件 vs Dumb 组件

根据是否需要高度的复用性，把组件划分为 Dumb 和 Smart 组件，约定俗成地把它们分别放到 components 和 containers 目录下

Dumb 组件只做一件事情：根据 props 进行渲染，而 Smart 负责应用的逻辑、数据，把所有相关的 Dumb、Smart 组件组合起来，通过 props 控制它们。

