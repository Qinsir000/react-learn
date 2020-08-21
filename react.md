# React

> **官网**：https://react.docschina.org/

## React 特点

- 轻量： react 的开发版多有的源码包括注释仅 3000 多行
- 原生：所有的 React 的代码都是用原生 JS 书写而成的，不依赖其他任何库
- 易扩展：React 对代码的封装程度较低，也没有过多的使用魔法，所以 React 中的很多功能都可以扩展。
- 不依赖宿主环境：React 只依赖原生 JS 语言，不依赖任何其他东西，包括运行环境。因此，它可以被轻松的移植到浏览器、桌面应用、移动端。
- 渐近式：React 并非框架，对整个工程没有强制约束力。这对与那些已存在的工程，可以逐步的将其改造为 React，而不需要全盘重写
- 单向数据流：所有的数据自顶而下的流动
- 用 JS 代码声明界面
- 组件化

## 对比 vue

| 对比项     | Vue | React |
| ---------- | --- | ----- |
| 全球使用量 |     | ✔     |
| 国内使用量 | ✔   |       |
| 性能       | ✔   | ✔     |
| 易上手     | ✔   |       |
| 灵活度     |     | ✔     |
| 大型企业   |     | ✔     |
| 中小型企业 | ✔   |       |
| 生态       |     | ✔     |

---

# React 基础

## React.createElement

创建一个 React 元素，称作虚拟 DOM，本质上是一个对象

1.  参数 1：元素类型，如果是字符串，一个普通的 HTML 元素
2.  参数 2：元素的属性，一个对象
3.  后续参数：元素的子节点

## ReactDOM.render

将 React.createElement 创建的 ReactDom 渲染到页面上

1. 参数 1： React.createElement 创建的 ReactDom
2. 绑定的页面元素

```
//创建一个span元素
        var span = React.createElement("span", {}, "一个span元素");
        //创建一个H1元素
        var h1 = React.createElement("h1", {
            title: "第一个React元素"
        }, "Hello", "World", span);
        ReactDOM.render(h1, document.getElementById("root"))
```

## 组件和组件属性

1. 函数组件

返回一个 React 元素

2. 类组件

继承 React.Component

render 函数

```
import React from 'react'
export default function MyFuncComp(props) {
    // return <h1>函数组件的内容</h1>
    return <h1>函数组件，目前的数字：{props.number}</h1>
}


export default class MyClassComp extends React.Component {
    render() {
        return <h1>类组件的内容，数字：{this.props.number}</h1>
    }
}
```

3. 组件的属性(props)

- 对于函数组件， 属性作为一个对象的属性，传递给函数的参数
- 对于类组件，属性会作为对象的属性，传递给构造函数的参数

**组件无法改变自身的属性**

4. 组件状态（state）
   自身可维护的数据

修改状态 this.setState({}) || this.setState(() => { return {} })

**组件中的数据**

- props：该数据是由组件的使用者传递的数据，所有权不属于组件自身，因此组件无法改变该数组
- state：该数组是由组件自身创建的，所有权属于组件自身，因此组件有权改变该数据

## setState

setState，它对状态的改变，**可能**是异步的

> 如果改变状态的代码处于某个 HTML 元素的事件中，则其是异步的，否则是同步
> 如果遇到某个事件中，需要同步调用多次，需要使用函数的方式得到最新状态

React 会对异步的 setState 进行优化，将多次 setState 进行合并（将多次状态改变完成后，再统一对 state 进行改变，然后触发 render）

```
// 利用setState第二个参数保证每次修改每次更新render
this.setState({
            n: this.state.n + 1
        }, () => {
            //状态完成改变之后触发，该回调运行在render之后
            console.log(this.state.n);
        });
```

## 生命周期

**生命周期仅存在于类组件中，函数组件每次调用都是重新运行函数，旧的组件即刻被销毁**

### 旧版生命周期

React < 16.0.0
1. constructor
   1.  同一个组件对象只会创建一次
   2.  不能在第一次挂载到页面之前，调用 setState，为了避免问题，构造函数中严禁使用 setState
2. componentWillMount
   1.  正常情况下，和构造函数一样，它只会运行一次
   2.  可以使用 setState，但是为了避免 bug，不允许使用，因为在某些特殊情况下，该函数可能被调用多次
3. **render**
   1.  返回一个虚拟 DOM，会被挂载到虚拟 DOM 树中，最终渲染到页面的真实 DOM 中
   2. render 可能不只运行一次，只要需要重新渲染，就会重新运行
   3.  严禁使用 setState，因为可能会导致无限递归渲染
4. **componentDidMount**
   1.  只会执行一次
   2.  可以使用 setState
   3.  通常情况下，会将网络请求、启动计时器等一开始需要的操作，书写到该函数中
5.  组件进入活跃状态
6. componentWillReceiveProps
   1.  即将接收新的属性值
   2.  参数为新的属性对象
   3.  该函数可能会导致一些 bug，所以不推荐使用
7. **shouldComponentUpdate**
   1.  指示 React 是否要重新渲染该组件，通过返回 true 和 false 来指定
   2.  默认情况下，会直接返回 true
8. componentWillUpdate
   1.  组件即将被重新渲染
9. componentDidUpdate
   1.   往往在该函数中使用 dom 操作，改变元素
10. **componentWillUnmount**
    1.   通常在该函数中销毁一些组件依赖的资源，比如计时器
![249d6d82b329fc758f045aa2a0b16a82.png](en-resource://database/500:1)

### 新版生命周期

React >= 16.0.0
React 官方认为，某个数据的来源必须是单一的
1. getDerivedStateFromProps
   1.  通过参数可以获取新的属性和状态
   2.  该函数是静态的
   3.  该函数的返回值会覆盖掉组件状态
   4.  该函数几乎是没有什么用
2. getSnapshotBeforeUpdate
   1.  真实的 DOM 构建完成，但还未实际渲染到页面中。
   2.  在该函数中，通常用于实现一些附加的 dom 操作
   3.  该函数的返回值，会作为 componentDidUpdate 的第三个参数

![1269628e20927eb61184b053776c785b.png](en-resource://database/499:1)

## 受控组件和非受控组件

受控组件：组件的使用者，有能力完全控制该组件的行为和内容。通常情况下，受控组件往往没有自身的状态，其内容完全收到属性的控制。
**完全由组件使用者控制**

非受控组件：组件的使用者，没有能力控制该组件的行为和内容，组件的行为和内容完全自行控制。
**有自己的控制范围**

**表单组件，默认情况下是非受控组件，一旦设置了表单组件的 value 属性，则其变为受控组件(单选和多选框需要设置 checked)**

## ref

获取相应的 dom 元素

React.createRef 函数创建方式创建一个 ref

### ref 转发

forwardRef 方法：
1.  参数，传递的是函数组件，不能是类组件，并且，函数组件需要有第二个参数来得到 ref
2.  返回值，返回一个新的组件

```

import React from 'react'
function A(props, ref) {
    return <h1 ref={ref}>
        组件A
        <span>{props.words}</span>
    </h1>
}
//传递函数组件A，得到一个新组件NewA
const NewA = React.forwardRef(A);
export default class App extends React.Component {
    ARef = React.createRef()
    componentDidMount() {
        console.log(this.ARef);
    }
    render() {
        return (
            <div>
                <NewA ref={this.ARef} words="asfsafasfasfs" />
                {/* this.ARef.current:  h1 */}
            </div>
        )
    }
}

```

### PureComponent

React.PureComponent 与 React.Component 几乎完全相同，但 React.PureComponent 通过 props 和 state 的浅对比来实现 shouldComponentUpate()。

在 PureComponent 中，如果包含比较复杂的数据结构，可能会因深层的数据不一致而产生错误的否定判断，导致界面得不到更新。

如果定义了 shouldComponentUpdate()，无论组件是否是 PureComponent，它都会执行 shouldComponentUpdate 结果来判断是否 update。如果组件未实现 shouldComponentUpdate() ，则会判断该组件是否是 PureComponent，如果是的话，会对新旧 props、state 进行 shallowEqual 比较，一旦新旧不一致，会触发 update。

**浅对比**：通过遍历对象上的键执行相等性，并在任何键具有参数之间不严格相等的值时返回 false。 当所有键的值严格相等时返回 true
。

**区别点**：PureComponent 自带通过 props 和 state 的浅对比来实现 shouldComponentUpate()，而 Component 没有。PureComponent 缺点可能会因深层的数据不一致而产生错误的否定判断，从而 shouldComponentUpdate 结果返回 false，界面得不到更新。PureComponent 优势不需要开发者自己实现 shouldComponentUpdate，就可以进行简单的判断来提升性能。

### render props

有时候，某些组件的各种功能及其处理逻辑几乎完全相同，只是显示的界面不一样，建议下面的方式认选其一来解决重复代码的问题（横切关注点）
1. render props
   1.  某个组件，需要某个属性
   2.  该属性是一个函数，函数的返回值用于渲染
   3.  函数的参数会传递为需要的数据
   4.  注意纯组件的属性（尽量避免每次传递的 render props 的地址不一致）
   5.  通常该属性的名字叫做 render
2. HOC（高阶组件，和高阶函数差不多）传入一个组件返回一个新组件，给组件包装了一下

高阶组件 HOC
![274a88adcae579d9205a25b9c3a03ee6.png](en-resource://database/526:0)

render props
![9aa8a250d464d2797446a2093821d77d.png](en-resource://database/528:0)

## Context

**创建上下文**
上下文是一个独立于组件的对象，该对象通过 React.createContext(默认值)创建
返回的是一个包含两个属性的对象
1. Provider 属性：生产者。一个组件，该组件会创建一个上下文，该组件有一个 value 属性，通过该属性，可以为其数据赋值
     同一个 Provider，不要用到多个组件中，如果需要在其他组件中使用该数据，应该考虑将数据提升到更高的层次
**使用上下文中的数据**
1.  在类组件中，直接使用 this.context 获取上下文数据
   1.  要求：必须拥有静态属性  contextType ,  应赋值为创建的上下文对象
2.  在函数组件中，需要使用 Consumer 来获取上下文数据
   1. Consumer 是一个组件
   2.  它的子节点，是一个函数（它的 props.children 需要传递一个函数）
**注意**
如果，上下文提供者（Context.Provider）中的 value 属性发生变化(Object.is 比较)，会导致该上下文提供的所有后代元素全部重新渲染，无论该子元素是否有优化（无论 shouldComponentUpdate 函数返回什么结果）

```
import React, { Component } from 'react'
const ctx = React.createContext();
class ChildB extends React.Component {
    static contextType = ctx;
    render() {
        console.log("childB render");
        return (
            <h1>
                a:{this.context.a}，b:{this.context.b}
            </h1>
        );
    }
}
export default class NewContext extends Component {
    state = {
        ctx: {
            a: 0,
            b: "abc",
            changeA: (newA) => {
                this.setState({
                    a: newA
                })
            }
        }
    }
    render() {
        return (
            <ctx.Provider value={this.state.ctx}>
                <div>
                    <ChildB />
                    <button onClick={() => {
                        this.setState({})
                    }}>父组件的按钮，a加1</button>
                </div>
            </ctx.Provider>
        )
    }
}


```

**注** 例子官网上有，这只是个总结

###  Portals

插槽：将一个 React 元素渲染到指定的 DOM 容器中

ReactDOM.createPortal(React 元素,  真实的 DOM 容器)，该函数返回一个 React 元素

![44b51a3827884f50200e03a49cbed7a1.png](en-resource://database/530:0)

**注意事件冒泡**
1. React 中的事件是包装过的
2.  它的事件冒泡是根据虚拟 DOM 树来冒泡的，与真实的 DOM 树无关。

### 错误边界

默认情况下，若一个组件在**渲染期间**（render）发生错误，会导致整个组件树全部被卸载

错误边界：是一个组件，该组件会捕获到渲染期间（render）子组件发生的错误，并有能力阻止错误继续传播

**让某个组件捕获错误**
1.  编写生命周期函数  getDerivedStateFromError
   1.  静态函数
   2.  运行时间点：渲染子组件的过程中，发生错误之后，在更新页面之前
   3. **注意：只有子组件发生错误，才会运行该函数**
   4.  该函数返回一个对象，React 会将该对象的属性覆盖掉当前组件的 state
   5.  参数：错误对象
   6.  通常，该函数用于改变状态
2.  编写生命周期函数  componentDidCatch
   1.  实例方法
   2.  运行时间点：渲染子组件的过程中，发生错误，更新页面之后，由于其运行时间点比较靠后，因此不太会在该函数中改变状态
   3.  通常，该函数用于记录错误消息

![e0cdc78c2318454cbd6dbfd92ebf818f.png](en-resource://database/532:0)

**注意**
某些错误，错误边界组件无法捕获
1.  自身的错误
2.  异步的错误
3.  事件中的错误
总结：仅处理渲染子组件期间的同步错误

### 渲染原理

渲染：生成用于显示的对象，以及将这些对象形成真实的 DOM 对象
- React 元素：React Element，通过 React.createElement 创建（语法糖：JSX）
  -  例如：
  - `<div><h1>标题</h1></div>`
  - `<App />`
- React 节点：专门用于渲染到 UI 界面的对象，React 会通过 React 元素，创建 React 节点，ReactDOM 一定是通过 React 节点来进行渲染的
-  节点类型：
  - React DOM 节点：创建该节点的 React 元素类型是一个字符串
  - React  组件节点：创建该节点的 React 元素类型是一个函数或是一个类
  - React  文本节点：由字符串、数字创建的
  - React  空节点：由 null、undefined、false、true
  - React  数组节点：该节点由一个数组创建
-  真实 DOM：通过 document.createElement 创建的 dom 元素

![f49f58ed01847c15828ce31cf4d70215.png](en-resource://database/534:0)

####   首次渲染(新节点渲染)

1.  通过参数的值创建节点
2.  根据不同的节点，做不同的事情
   1.  文本节点：通过 document.createTextNode 创建真实的文本节点
   2.  空节点：什么都不做
   3.  数组节点：遍历数组，将数组每一项递归创建节点（回到第 1 步进行反复操作，直到遍历结束）
   4. DOM 节点：通过 document.createElement 创建真实的 DOM 对象，然后立即设置该真实 DOM 元素的各种属性，然后遍历对应 React 元素的 children 属性，递归操作（回到第 1 步进行反复操作，直到遍历结束）
   5.  组件节点
      1.  函数组件：调用函数(该函数必须返回一个可以生成节点的内容)，将该函数的返回结果递归生成节点（回到第 1 步进行反复操作，直到遍历结束）
      2.  类组件：
         1.  创建该类的实例
         2.  立即调用对象的生命周期方法：static getDerivedStateFromProps
         3.  运行该对象的 render 方法，拿到节点对象（将该节点递归操作，回到第 1 步进行反复操作）
         4.  将该组件的 componentDidMount 加入到执行队列（先进先出，先进先执行），当整个虚拟 DOM 树全部构建完毕，并且将真实的 DOM 对象加入到容器中后，执行该队列
3.  生成出虚拟 DOM 树之后，将该树保存起来，以便后续使用
4.  将之前生成的真实的 DOM 对象，加入到容器中。

```js
const app = (
  <div className="assaf">
        
    <h1>
              标题         {["abc", null, <p>段落</p>]}
          
    </h1>
        <p>
              {undefined}
          
    </p>
  </div>
);
ReactDOM.render(app, document.getElementById("root"));
```

以上代码生成的虚拟 DOM 树：
![f49f58ed01847c15828ce31cf4d70215.png](en-resource://database/534:1)

```js
function Comp1(props) {
  return <h1>Comp1 {props.n}</h1>;
}
function App(props) {
  return (
    <div>
                  
      <Comp1 n={5} />
              
    </div>
  );
}
const app = <App />;
ReactDOM.render(app, document.getElementById("root"));
```

以上代码生成的虚拟 DOM 树：
![d9609fad2cb09cd543b1fb0608fa7ce9.png](en-resource://database/536:0)

```js
class Comp1 extends React.Component {
  render() {
    return <h1>Comp1</h1>;
  }
}
class App extends React.Component {
  render() {
    return (
      <div>
                        
        <Comp1 />
                    
      </div>
    );
  }
}
const app = <App />;
ReactDOM.render(app, document.getElementById("root"));
```

以上代码生成的虚拟 DOM 树：

![37469debc0e8ae69b3a5aed75d04aebe.png](en-resource://database/538:0)

####   更新节点

更新的场景：
1.  重新调用 ReactDOM.render，触发根节点更新
2.  在类组件的实例对象中调用 setState，会导致该实例所在的节点更新
**节点的更新**
-  如果调用的是 ReactDOM.render，进入根节点的**对比（diff）更新**
-  如果调用的是 setState
  - 1.  运行生命周期函数，static getDerivedStateFromProps
  - 2.  运行 shouldComponentUpdate，如果该函数返回 false，终止当前流程  
  - 3.  运行 render，得到一个新的节点，进入该新的节点的**对比更新**
  - 4.  将生命周期函数 getSnapshotBeforeUpdate 加入执行队列，以待将来执行
  - 5.  将生命周期函数 componentDidUpdate 加入执行队列，以待将来执行

后续步骤：
1.  更新虚拟 DOM 树
2.  完成真实的 DOM 更新
3.  依次调用执行队列中的 componentDidMount
4.  依次调用执行队列中的 getSnapshotBeforeUpdate
5.  依次调用执行队列中的 componentDidUpdate

####   对比更新

将新产生的节点，对比之前虚拟 DOM 中的节点，发现差异，完成更新
问题：对比之前 DOM 树中哪个节点
React 为了提高对比效率，做出以下假设
1.  假设节点不会出现层次的移动（对比时，直接找到旧树中对应位置的节点进行对比）
2.  不同的节点类型会生成不同的结构
   1.  相同的节点类型：节点本身类型相同，如果是由 React 元素生成，type 值还必须一致
   2.  其他的，都属于不相同的节点类型
3.  多个兄弟通过唯一标识（key）来确定对比的新节点
key 值的作用：用于通过旧节点，寻找对应的新节点，如果某个旧节点有 key 值，则其更新时，会寻找相同层级中的相同 key 值的节点，进行对比。
**key 值应该在一个范围内唯一（兄弟节点中），并且应该保持稳定**
####  找到了对比的目标
判断节点类型是否一致
- **一致**
根据不同的节点类型，做不同的事情
**空节点**：不做任何事情
**DOM 节点**：
1.  直接重用之前的真实 DOM 对象
2.  将其属性的变化记录下来，以待将来统一完成更新（现在不会真正的变化）
3.  遍历该新的 React 元素的子元素，**递归对比更新**
**文本节点**：
1.  直接重用之前的真实 DOM 对象
2.  将新的文本变化记录下来，将来统一完成更新
**组件节点**：
**函数组件**：重新调用函数，得到一个节点对象，进入**递归对比更新**
**类组件**：
1.  重用之前的实例
2.  调用生命周期方法 getDerivedStateFromProps
3.  调用生命周期方法 shouldComponentUpdate，若该方法返回 false，终止
4.  运行 render，得到新的节点对象，进入**递归对比更新**
5.  将该对象的 getSnapshotBeforeUpdate 加入队列
6.  将该对象的 componentDidUpdate 加入队列
**数组节点**：遍历数组进行**递归对比更新**
- **不一致**
整体上，卸载旧的节点，全新创建新的节点
**创建新节点**
进入新节点的挂载流程
**卸载旧节点**
1. **文本节点、DOM 节点、数组节点、空节点、函数组件节点**：直接放弃该节点，如果节点有子节点，递归卸载节点
2. **类组件节点**：
   1.  直接放弃该节点
   2.  调用该节点的 componentWillUnMount 函数
   3.  递归卸载子节点

####   没有找到对比的目标

新的 DOM 树中有节点被删除
新的 DOM 树中有节点添加
-  创建新加入的节点
-  卸载多余的旧节点

## hook

###  useState

useState
-  函数有一个参数，这个参数的值表示状态的默认值
-  函数的返回值是一个数组，该数组一定包含两项
  -  第一项：当前状态的值
  -  第二项：改变状态的函数

**注意**
1.  使用函数改变数据，传入的值不会和原来的数据进行合并，而是直接替换。
2.  如果要实现强制刷新组件
   1.  类组件：使用 forceUpdate 函数
   2.  函数组件：使用一个空对象的 useState

```

import React, { useState } from 'react'
export default function App() {
    console.log("App Render");
    const [visible, setVisible] = useState(true);
    const [n, setN] = useState(0);
    return <div>
        <p style={{ display: visible ? "block" : "none" }}>
            <button onClick={() => {
                setN(n - 1)
            }}>-</button>
            <span>{n}</span>
            <button onClick={() => {
                setN(n + 1)
            }}>+</button>
        </p>
        <button onClick={() => {
            setVisible(!visible);
        }}>显示/隐藏</button>
    </div>
}


// 类组件强制刷新
import React, { Component, useState } from 'react'
export default class App extends Component {
    render() {
        return (
            <div>
                <button onClick={()=>{
                    //不会运行shouldComponentUpdate
                    this.forceUpdate();//强制重新渲染
                }}>强制刷新</button>
            </div>
        )
    }
}
// hook 强制刷新
export default function App() {
    console.log("App Render");
    const [, forceUpdate] = useState({});
    return <div>
        <p >
            <button onClick={() => {
                forceUpdate({});
            }}>强制刷新</button>
        </p>
    </div>
}
```

### useEffect

useEffect: 用于处理副作用的
副作用：
1. ajax 请求
2.  计时器
3.  其他异步操作
4.  更改真实 DOM 对象
5.  本地存储
6.  其他会对外部产生影响的操作
函数：useEffect，该函数接收一个函数作为参数，接收的函数就是需要进行副作用操作的函数

```
// 无需清除的effect
import React, { useState, useEffect } from 'react';function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
        document.title = `You clicked ${count} times`;
  });
  return (
    <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>
             Click me
         </button>
    </div>
  );}
```

**注意**
1.  副作用函数的运行时间点，是在页面完成真实的 UI 渲染之后。因此它的执行是异步的，并且不会阻塞浏览器
   1.  与类组件中 componentDidMount 和 componentDidUpdate 的区别
   2. componentDidMount 和 componentDidUpdate，更改了真实 DOM，但是用户还没有看到 UI 更新，同步的。
   3. useEffect 中的副作用函数，更改了真实 DOM，并且用户已经看到了 UI 更新，异步的。
2.  每个函数组件中，可以多次使用 useEffect，但不要放入判断或循环等代码块中。
3. useEffect 中的副作用函数，可以有返回值，返回值必须是一个函数，该函数叫做清理函数
   1.  该函数运行时间点，在每次运行副作用函数之前
   2.  首次渲染组件不会运行
   3.  组件被销毁时一定会运行

4. useEffect 函数，可以传递第二个参数
   1.  第二个参数是一个数组
   2.  数组中记录该副作用的依赖数据
   3.  当组件重新渲染后，只有依赖数据与上一次不一样的时，才会执行副作用
   4.  所以，当传递了依赖数据之后，如果数据没有发生变化
      1.  副作用函数仅在第一次渲染后运行
      2.  清理函数仅在卸载组件后运行
5.  副作用函数中，如果使用了函数上下文中的变量，则由于闭包的影响，会导致副作用函数中变量不会实时变化。
6.  副作用函数在每次注册时，会覆盖掉之前的副作用函数，因此，尽量保持副作用函数稳定，否则控制起来会比较复杂。

```
import React, { useState, useEffect } from 'react'
function Test() {
    useEffect(() => {
        console.log("副作用函数，仅挂载时运行一次")
        return () => {
            console.log("清理函数，仅卸载时运行一次")
        };
    }, []); //使用空数组作为依赖项，则副作用函数仅在挂载的时候运行
    console.log("渲染组件");
    const [, forceUpdate] = useState({})
    return <h1>Test组件 <button onClick={() => {
        forceUpdate({})
    }}>刷新组件</button></h1>
}
export default function App() {
    const [visible, setVisible] = useState(true)
    return (
        <div>
            {
                visible && <Test />
            }
            <button onClick={() => {
                setVisible(!visible);
            }}>显示/隐藏</button>
        </div>
    )
}

```

## 结尾

react 内容不止这些，只是先把自己认为重要的先整理了一下，其他内容会慢慢补充。
