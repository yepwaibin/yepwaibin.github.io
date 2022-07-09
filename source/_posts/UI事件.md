---
title: UI事件
date: 2022-03-18 21:16:44
tags: UI事件
categories: 现代Javascript教程
---

## 鼠标事件

- mousedown/moseup
  - 点击/释放
- mouseover/mouseout
  - 移入/移出
- mousemove
  - 鼠标移动
- click
- dbclick
  - 双击
- contextmenu
  - 右键触发

<!--more-->

### 鼠标按钮

event.button

- 左键-0
- 中键-1
- 右键-2
- 后退按键-3
- 前进按键-4

### 组合键

- shiftKey ： Shift
- altKey ： Alt （或对于 Mac 是 Opt ）
- ctrlKey ： Ctrl
- metaKey ：对于 Mac 是 Cmd

### 坐标

- 相对于窗口的坐标： clientX 和 clientY 
  - 以窗口左上角做参照物
- 相对于文档的坐标： pageX 和 pageY 
  - 以文档左上角做参照物

### 防止复制

oncopy="return false"

## 移动鼠标

### 事件 mouseover 和 mouseout 

当鼠标指针移到某个元素上时， mouseover 事件就会发生，而当鼠标离开该元素 时， mouseout 事件就会发生。注意：快速移动鼠标可能会跳过中间元素

![鼠标事件图](UI事件/8844cdf43e0eae86ff31997af9fada4.png)

- 对于mouseover
  - event.target —— 是鼠标移过的那个元素
  - event.relatedTarget —— 是鼠标来自的那个元素（ relatedTarget → target ）
- 对于mouseout
  - event.target —— 是鼠标离开的元素
  - event.relatedTarget —— 是鼠标移动到的，当前指针位置下的元素 （ target → relatedTarget ）

如果 mouseover 被触发了，则必须有 mouseout

#### 嵌套元素

当鼠标指针从` #parent` 元素移动到` #child `时，会在父元素上触发两 个处理程序：` mouseout `和 `mouseover`。因为时间会冒泡，子元素触发mouseover事件，向上冒泡，父元素就会捕获到。

### 事件 mouseenter 和 mouseleave

与前面事件 mouseover 和 mouseout 区别

- 元素内部与后代之间的转换不会产生影响
- 事件 mouseenter/mouseleave 不会冒泡

## 鼠标拖放事件

事件流： ball.mousedown → document.mousemove → ball.mouseup （不要忘记取消原生 ondragstart ）
