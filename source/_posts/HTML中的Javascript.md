---
title: HTML中的Javascript
date: 2022-01-06 22:59:57
tags: Javascript
categories: Javascript高级程序设计
---

## `<script>`元素

- async

- crossorigin

  > 配置相关CORS设置

  - anonymous
  - use-credentials

- defer

- intergrity

  > 允许比对接收到的资源和指定的加密签名以验证子资源完整性，如果接收到的资源的签名与这个属性指定的签名不匹配，则页面会报错，脚本不会执行，用于确保内容分发网络（CDN，ContentDeliveryNetwork）不会提供恶意内容

- src

  - 会忽略行内脚本
  - 不会受浏览器的同源策略限制，但返回并被执行的JavaScript则受限制

- type

  - `text/javascript`
  - `module`
    - 会被当成ES6模块
    - 脚本加载解析过程与defer相同

<!--more-->

## 行内脚本与外部脚本的比较

1. 行内脚本

   下载和执行会阻塞`HTML`的解析

2. 外部脚本

   - 不加`async`和`defer`
     - 与行内脚本一样，下载和执行会阻塞HTML的解析
   - `defer`
     - 下载不会阻塞，执行放在最后，执行会有先后顺序，且会在`DOMContentLoaded`事件之前执行
   - `async`
     - 下载不会阻塞，下载完后会马上执行，且执行并不保证能按照它们出现的次序执行
     - 异步脚本保证会在页面的`load`事件前执行，但可能会在`DOMContentLoaded`之前或之后

3. 动态加载脚本

   通过`document.createElement`动态加载，会异步加载，相当于添加`async`属性

建议都使用外部脚本，原因：

- 可维护性
- 缓存
  - 浏览器会根据特定的设置缓存所有外部链接的JavaScript文件，如果两个页面都用到同一个文件，则该文件只需下载一次。

## 文档模式对Javascript有什么影响

可以使用`doctype`切换文档模式，共有三种文档模块

1. 混杂模式
2. 标准模式
3. 准标准模式

前两种主要区别体现在通过CSS渲染的内容方面，后两种主要区别在于如何对待图片元素周围的空白（在表格中使用图片时最明显）。

## 确保Javascript不可用时的用户体验

使用`<noscript>`元素，在JavaScript被禁用或不支持时，元素里面的内容会呈现出来。`<noscript>`元素可以包含任何可以出现在`<body>`中的HTML元素。