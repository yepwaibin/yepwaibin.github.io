---
title: React
date: 2022-01-21 02:35:50
tags: React
categories: 青训营
---

## 历史和应用

- 一个东西的出现，往往是因为在某个历史阶段人们产生了某些需求
- 新的东西出现肯定是因为之前的东西不好用

<!--more-->

### React的原型：FaxJS

- 客户端或服务端渲染
- 响应式
  - 状态更新->UI更新
- 性能好
- 结构化

[Tom Occhino and Jordan Walke: JS Apps at Facebook](https://www.youtube.com/watch?v=GW0rj4sNH2w)

## 设计思路

例子：苹果官网手机价格变化

### UI编程痛点

- 状态更新，UI不会自动更新，需要手动调用DOM进行更新
  - 需改良：状态更新，UI自动更新
- 欠缺基本的代码层面的封装和隔离，代码层面没有组件化
  - 需改良：前端代码组件化，可复用，可封装
- UI之间的数据依赖关系需要手动维护，如果依赖链路长，则会遇到回调地狱
  - 需改良：状态之间互相依赖关系，只需声明即可

例子：火警，烧水

### 响应式编程

1. 事件
2. 执行既定的回调
3. 状态变更
4. UI更新

回到苹果官网例子，对页面进行组件化划分

- 组件是 组件的组合/原子组件
- 组件内拥有状态，外部不可见
- 父组件可将状态传入组件内部

### 状态归属问题

- 确定“数据”应该放在哪个组件里保存
- 状态归属于两个节点向上寻找到最近的祖宗节点
  - 又会带来一个问题，数据有时经常性放在很上层的组件上，这会失去了组件复用的初衷
  - 子组件如何触发改变上层组件数据的事件？
    - 通过上层组件传递函数到子组件，子组件再调用此函数

### 思考

1. React是单向数据流还是双向数据流
   - 是单向数据流
   - 父组件能把状态传给子组件，而子组件并不能把状态网上传给父组件，只是单纯的执行了父组件传下来的函数，从而能改变了父组件的状态
2. 如何解决状态不合理上升的问题
3. 组件的状态改变后，如何更新DOM

状态应是局部性

### 组件设计

1. 组件声明了状态和UI的映射
2. 组件有Props/State两种状态
   - 内部拥有私有状态State
   - 外部能接受Props状态，提供复用性
   - 根据当前State/Props返回一个UI
3. “组件”可由其他组件拼接而成

### 生命周期

1. 组件挂载时

   - [**`constructor()`**](https://zh-hans.reactjs.org/docs/react-component.html#constructor)

   - [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)

   - [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)

   - [**`componentDidMount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidmount)

2. 组件更新时

   - [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
   - [`shouldComponentUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)
   - [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)
   - [`getSnapshotBeforeUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
   - [**`componentDidUpdate()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate)

3. 组件卸载时

   - [**`componentWillUnmount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentwillunmount)

## hooks写法

副作用

- 改变了外部的东西

## 实现

### 实现时存在的三个问题

1. JSX不符合JS标准
2. 返回的JSX发生改变时如何更新DOM
3. State/Props更新时，要重新触发render函数（render函数就是组件函数）

### 解决方法

1. 转义JSX
2. 使用虚拟DOM
   - 虚拟DOM是一种用于和真实DOM同步，而在JS内存中维护的一个对象，它具有和DOM类似的树状结构，并和DOM可以建立一一对应的关系
   - 真实的DOM不是JS内存中的对象，而是浏览器维护的一个状态
3. diff算法更新DOM
   - 我们只能通过DOM接口去修改DOM，而不能直接去修改DOM。但是我们可以声明一个变量，和真实DOM有类似结构的变量，每次更新的时候，都生成一个新的虚拟DOM，再使用diff算法去对比新旧虚拟DOM，再去修改真实DOM

插入一个问题：既然所有前端框架都是声明式的，那为什么不把声明式直接植入到浏览器中，自带声明式，这样就不用框架了。

- 因为，浏览器自己作为应用平台，不能只提供更高层的东西，因为这样会把浏览器的自由度变得更低了。

### diff算法核心

1. 不同类型的元素->替换
2. 同类型的DOM元素->更新
3. 同类型的组件元素->递归

更新次数少与计算速度快的抉择

用层次遍历，每个节点只遍历一次，时间复杂度`O(n^3)`降到`O(n)`

出现一个问题：但父组件的状态发生改变，子组件和嵌套的子组件都要去递归的发生改变，其实这样很耗费性能，所以要考虑如何去优化。

### 编程思想

- 指令式编程
  - 一步步写清楚
- 声明式编程
  - 只需说明你的目的
- 响应式编程
  - 是声明式编程的一个类别
  - 不仅像声明式编程一样，还可以自动更新UI

## 状态管理库

> 将状态抽离的UI外部进行统一管理

问题：假如所有数据都放到外部的Store中，那么会与外部store发生强耦合，可复用性就变差。所以要如何选择哪些状态放到Store，哪些状态只放在自身组件中。

可以把Store看成这样：这个store是多个组件（把这多个组件看成一个大的整体组件）内部的一个state

### 状态机

当前状态，收到外部事件，前一道下一个状态

## 应用级框架科普

- next
  - 了解一间公司vercel
- modernJs
  - 字节的全栈开发框架
- Blitz
  - 无API思想的全栈开发框架
