---
title: 什么是Javascript
date: 2022-01-05 22:49:10
tags: Javascript
categories: Javascript高级程序设计
---

## Javascript历史回顾

>  最开始是为了解决验证简单的表单需要大量与服务器的往返进行通信，由客户端处理输入验证

最早有两个版本的Javascript

1. 网景的Javascript
2. 微软IE的JScript

由于有两个版本的并存，急需要对Javascript进行规范其语法或特性标准，所以多家厂商联合，发布了`ECMA-262`，也就是`ECMAScript`这个脚本语法标准

<!--more-->

## Javascript是什么

包含三个部分

### ECMAScript

> `ECMAScript`是`ECMA-262`定义的语言，并不局限在Web浏览器

Web浏览器、NodeJs都只是`ECMAScript`实现的一种宿主环境。在基本层面上，`ECMA-262`定义了这门语言的语法、类型、语句、关键字、保留字、操作符、全局对象。

#### `ECMAScript`的主要版本更新

- 第一版与网景的Javascript1.1基本相同，删除了所有浏览器特定的代码
- 第三版更新了字符串处理、错误定义、数值定义，增加正则表达式、新的控制语句、`try/catch`异常处理的支持
- 第四版修改颇多，几乎定义了一个新的语言废弃
- 第五版就是3.1版，增加原生的解析和序列化JSON数据的JSON对象、方便基乘和高级属性定义的方法
- 第六版，俗称ES6，正式支持类、模块、迭代器、生成器、箭头函数、期约、反射、代理和众多新的数据类型
- 第八版，增加异步函数(`async/await`)，`Object.values()/Object.entries()/Object.getOwnPropertyDescriptors()`和字符串填充方法，明确对象字面量最后的逗号
- 第九版，异步迭代、剩余、扩展属性，新的正则表达式特性、`Promisefinally()`，模板字面量修订
- 第十版，`Array.prototype.flat()/flatMap()、String.prototype.trimStart()/trimEnd()、Object.fromEntries()`方法，以及`Symbol.prototype.description`属性，定义`Function.prototype.toString()`返回值，并固定`Array.prototype.sort()`的顺序，解决JSON字符串兼容的问题。定义catch子句的可选绑定。

### DOM

> 使用`ECMAScript`的核心类型和语法，提供与环境的额外功能，是一个应用编程接口（API），用于在HTML中使用扩展的XML

DOM通过创建表示文档的树，通过DOMAPI进行增删查改节点，可以做到不刷新页面而修改页面外观和内容。

DOM三个Level

1. Level1目标是映射文档结构
2. Level2新增模块
   - DOM视图
   - DOM事件
   - DOM样式
   - DOM遍历和范围
3. Level3增加了以统一的方式加载和保存文档的方法（包含在一个叫DOMLoadandSave的新模块中），还有验证文档的方法（DOMValidation）。

###  BOM

> 用于支持访问和操作浏览器窗口，操控浏览器显示页面之外的部分，主要针对浏览器窗口和子窗口

- 弹出新浏览器窗口的能力
- 移动、缩放和关闭浏览器窗口的能力
- navigator对象，提供关于浏览器的详尽信息
- location对象，提供浏览器加载页面的详尽信息
- screen对象，提供关于用户屏幕分辨率的详尽信息
- performance对象，提供浏览器内存占用、导航行为和时间统计的详尽信息
- 对cookie的支持
- 其他自定义对象，如XMLHttpRequest和IE的ActiveXObject

## Javascript与ECMASCript的关系

`ECMAScript`只是对`ECMA-262`实现规范描述的所有方面的一门语言的称呼，而Javascript实现了`ECMAScript`，Javascript是包含`ECMAScript`

原则上`JavaScript`与`ECMAScript`指的是同一个东西，但有时也会加以区分

- `JavaScript`：指语言及其实现
- `ECMAScript`：指语言标准及语言版本