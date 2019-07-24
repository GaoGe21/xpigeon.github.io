---
title: React生命周期
date: 2019-05-03 22:04:17
tags: study
---

-------------------
react组件的生命周期分为三部分：
1.  实例化
2.  挂载阶段
3.  卸载阶段


- **实例化** 

| 组件在客户端被实例化 | 组件在服务端被实例化 | | 
| :-------- | --------:|--------:|
| constructor(props)   | constructor(props)  | 
| getDefaultProps   | getDefaultProps  | 只调一次，可设置默认值|
| getInitialState   | getInitialState  | 每个实例都只调一次，初始化state
| componentWillMount | componentWillMount  |  （异步渲染不安全）
| render | render  | 创建虚拟DOM |
| componentDidMount |   |  调用该时已访问到真实DOM


由于组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM （virtual DOM）。只有当它插入文档以后，才会变成真实的 DOM 。有时需要从组件获取真实 DOM 的节点，这时要用到 ref 属性。


	var Area = React.createClass({
	    render: function(){
	        this.getDOMNode(); //render调用时，组件未挂载，这里将报错
        return <canvas ref='mainCanvas'>
	    },
	    componentDidMount: function(){
	        var canvas = this.refs.mainCanvas.getDOMNode();
	        //这是有效的，可以访问到 Canvas 节点
	    }
	})

- **挂载阶段** 

此时组件已经渲染好并且用户可以与它进行交互。当应用状态发生改变时，依次调用一下方法：

**1、componentWillReceiveProps**
组件的 props 属性可以通过父组件来更改。可以在这个方法里更新 state，以触发 render 方法重新渲染组件。

**2、shouldComponentUpdate**
此方法返回布尔值控制组件是否重新渲染（基本不在开发中使用）
（当执行this.forceUpdate时，shouldComponentUpdate将不会被触发）

**3、componentWillUpdate**
在组件接收到了新的 props 或者 state 即将进行重新渲染前调用。
（注意不要在此方面里再去更新 props 或者 state！！）

**4、render**

**5、componentDidUpdate**
在组件重新被渲染之后调用，在此处访问并修改DOM。


- **卸载阶段** 

**1、componentWillUnmount**

当React使用完一个组件，此组件必须从 DOM 中卸载后被销毁，执行 componentWillUnmout 完成所有的清理和销毁工作。

当再次装载组件时，以下方法会被依次调用：

getInitialState => componentWillMount => render => componentDidMount

-------------------

**关于 constructor()**
constructor()是在React 组件在挂载之前调用的方法，当创建的组件extends React.Component，在调用constructor()方法内必须先调用 **super(props)** 来调用基类的构造方法 constructor()， 也将父组件的props注入给子组件，供子组件读取(组件中props只读不可变，state可变)。

-------------------

React v16.3引入了两个新的生命周期函数以及一个错误捕获函数

**getDerivedStateFromProps**
static getDerivedStateFromProps(props, state) 在组件创建时和更新时的render方法之前调用，它应该返回一个对象来更新状态，或者返回null来不更新任何内容。

**getSnapshotBeforeUpdate**
getSnapshotBeforeUpdate() 被调用于render之后，可以读取但无法使用DOM的时候。它使您的组件可以在可能更改之前从DOM捕获一些信息（例如滚动位置）。此生命周期返回的任何值都将作为参数传递给componentDidUpdate（）。

**componentDidCatch()**  
错误捕获