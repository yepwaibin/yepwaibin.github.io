---
title: HTML基础
date: 2022-01-18 15:52:09
tags: ['HTML', '面经']
categories: 青训营
---

## 前端关注点

- 解决GUI人机交互问题
- 跨终端
  - PC/移动浏览器
  - 客户端/小程序
  - VR/AR等
- Web技术栈

<!--more-->

## 前端技术栈

服务器端通过网络协议与客户端交流

而前端主要指下面三个部分

- Javascript（行为）
- CSS（样式）
- HTML（内容）

## 关注方面

- 功能
  - 首要部分

- 美观
- 无障碍
- 安全
  - 用户数据安全
- 性能
- 兼容性
  - 跨端

- 体验

## HTML是什么

> HyperText Markup Language

- HyperText
  - 图片、标题、链接、表格
- Markup Language
  - 标签

### DOCTYPE的作⽤

决定HTML使用哪个版本，并且浏览器根据这个来决定使用哪一种的渲染模式，有两种渲染模式

- 标准模式
  - 按照最佳的相关规范进行渲染
- 怪异模式
  - 一种比较宽松的向后兼容的方式显示

浏览器拿到HTML后，将其解析为DOM树

## HTML语法

- 标签和属性不区分大小写
- 空标签可以不闭合
- 属性值推荐用引号包裹
- 某些属性值可以省略

### 列表

- ol、li
- ul、li
- dl、dt、dd
  - 定义列表
  - 定义标题
  - 定义描述

### 链接

- a
  - href
    - 跳转链接位置
  - target
- img
  - src
  - alt
- audio
  - src
  - controls
    - 播放控件
- video
  - src
  - controls

### 输入

- input
  - placeholder
  - type
    - range
    - number
    - date
    - checkbox
    - radio
      - name属性设置相同属性值
- textarea
  - 多行输入
- select option
- datalist option
  - 提示补全

### 引用

- blockquote
- 长引用
  - cite属性，引用来源
- cite
  - 短引用
  - 作品、名字
- q
  - 短引用
  - 一句话
- code
  - 代码引用

### 文本标签

- strong
  - 表示事情非常重要、严重
- em
  - 语气上的强调

### 内容划分

- header
  - nav
- main
  - artticle
- aside
- footer

## 语义化

元素、属性、属性值都拥有某些含义

### 优点

- 开发者
  - 代码可读性
  - 可维护性
- 浏览器
- 搜索引擎
  - 提取关键词、排序
  - 搜索引擎优化
- 屏幕阅读器
  - 提升无障碍性

### 如何做到语义化

- 了解标签、属性含义
- 思考什么标签最适合描述这个内容

