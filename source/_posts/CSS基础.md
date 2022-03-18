---
title: CSS基础
date: 2022-01-18 15:48:39
tags: ['CSS', '面经']
categories: 青训营
typora-root-url: ..
---

> 用来定义页面元素的样式

- 设置字体颜色
- 位置、大小
- 添加动画效果

## 组成部分

样式规则

- 选择器
- 属性
- 属性值
- 声明
  - 属性+属性值

<!--more-->

## 样式来源

- 外联
- 内部
- 内联
  - 不需要写选择器

优先级：内联 > 内部样式 > 外联样式

## 选择器

| 选择器       | 用法                        | 优先级权重 |
| ------------ | --------------------------- | ---------- |
| 标签选择器   | `div`                       | 1          |
| 类名选择器   | `.classname`                | 10         |
| id选择器     | `#id`，需要唯一             | 100        |
| 属性选择器   | `[]`，可以做匹配            | 10         |
| 通配选择器   | `*`                         | 0          |
| 伪类选择器   | `:`，分为状态伪类和结构伪类 | 10         |
| 伪元素选择器 | `::`                        | 1          |

- `!important`声明优先级最高
- 优先级相同，后出现样式生效

## 组合方式

- `AB`（直接组合）
  - 满足A，并且满足B
- `A B`（后代组合）
  - 选中B，且B是A的后代
- `A > B`（亲子组合）
  - 选中B，且B是A的**子代**
- `A ~ B`（兄弟选择器）
  - 与A是同级，且在A后面的**所有**兄弟都会被选中
- `A + B`（相邻选择器）
  - 选中B，B紧跟在A后面，A、B在同一级上
- 选择器组
  - 逗号隔开

## CSS如何工作

解析HTML时，加载CSS，解析CSS，把样式添加到DOM节点

## 继承

某些属性会自动继承父元素的**计算值**，除非显示指定一个值

- 可以显性继承inherit

### 无继承属性

- 盒子模型属性
- 定位属性
  - `float`、`position`、上下左右、`min-（长宽）`、`max-（长宽）`
- 轮廓样式
  - `outline`、`outline-（style、width、color）`
- 。。。

### 有继承属性

- 字体属性
- 文本属性
  - `line-height`、`word-spacing`、`letter-spacing`、`text-indent`文本缩进
- 列表布局

## 盒模型

有两种，可以通过`box-sizing`设置

- 标准盒模型
  - `content-box`
  - `width`、`height`的数值只是内容的宽高
    - `height`，容器有指定高度时，百分数才生效
    - `width`，百分数相对于容器的content box宽度
- 怪异盒模型
  - `border-box`
  - `width`、`height`的数值等于（内容+内边距+边框）

### 盒模型组成

- `content`
- `padding`
  - 百分数相对容器宽度
- `border`
  - `border-weight`
  
  - `border-style`
  
  - `border-color`
  - 可以画三角形
- `margin`
  - `margin` 重叠合并
    - 只在垂直方向
  - `margin:auto`可以水平居中
  - 百分数相对于容器宽度

## 块级、行级区别

### 行级

- `display:inline`
- 和其他行级盒子放在一行或拆成多行
- 盒模型中的`width`和`height`属性不能用
- 可以设置水平方向的`margin`和`padding`属性，单不能设置垂直方向的`margin`和`padding`属性
- 只包含行级盒子的容器会创建一个IFC
  - 盒子在一行水平摆放
  - 一行放不下，换行显示
  - `text-align`决定一行内盒子的水平对齐
  - `vertical-align`决定垂直对齐
  - 会避开浮动元素

### 块级

- `display:block`
- 不和其他盒子并列摆放
- 使用所有的盒模型属性
- **某些**（不是所有块级盒子）容器会创建一个BFC
  - 根元素
  - 浮动、绝对定位、`inline-block`
  - Flex子项、Grid子项
  - `overflow`值不是`visible`
  - `display：flow-root`
  - BFC排版规则
    - 盒子从上到下拜访
    - **垂直**`margin`合并
    - BFC内，盒子的`margin`不会与外面的合并
    - BFC不会和浮动元素重叠

### inline-block

- 设置为`inline`对象，但呈现**内容**时作为`block`对象呈现
- 所以可以设置宽高

## 布局

- 确定内容的大小和位置的算法
- 依据元素、容器、兄弟节点和内容等信息来计算

### 三大类

#### 常规流

- > 大规则：块级从上到下，行级从左到右、根元素、浮动和绝对定位会脱离常规流

- 行级

- 块级

- 表格布局

- FlexBox

  - 控制摆放流向
  - 摆放顺序
  - 盒子宽带高度
  - 水平垂直对齐
  - 是否允许折行

- Grid布局

#### 浮动

最早用于文字环绕图片

#### 绝对定位

### 两栏布局实现

> 左定宽，右自适应

- 左浮动，右`margin-left`值等于左宽
- 左浮动，右设置`overflow: hidden`触发BFC
- 用flex，左定宽，右设置`flex: 1`，容器`display: flex`
- 用grid，容器`display: grid; grid-template-columns: 200px 1fr`
- 左绝对定位，右`margin-left`设为左的宽度，父级元素记得设为非`static`

### 三栏布局实现

详细请看[docsify文档网站](https://sweet-kk.github.io/css-layout/)

## 理解BFC（块级格式上下文）

具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

触发条件

- 根元素`：body`；
- `float `除` none `以外的值；
- `position (absolute、fixed)`；
- `display `值为：`inline-block`、`table-cell`、`table-caption`、`flex`等；
- `overflow` 值为：`hidden`、`auto`、`scroll`；

### 作用

- 解决`margin`重叠问题
- 解决高度坍塌问题
- 清除浮动
  - BFC区域不会与浮动的容器发生重叠

## flex语法及使用

### 容器

- `display:flex`，行内元素可以`display:inline-flex`
- `flex-direction`
  - `row,row-reverse,column,column-reverse`
- `flex-wrap`
  - `nowrap,wrap,wrap-reverse`
- `justify-content`
  - `flex-start,flex-end,center,space-between(两边靠边),space-around`
- `align-items`
  - `flex-start,flex-end,center,baseline,stretch(默认值，将占满整个容器高度)`
- `align-content`
  - 多个轴线对齐方式，只有一根，属性不起作用
  - `flex-start,flex-end,center,space-between,space-around,stretch`

### 子项

- order
  - 排列顺序，数值越小，排列越靠前，默认为0
- flex-grow
  - 放大比例，默认0，即如果存在剩余空间，也不放大
- flex-shrink
  - 缩小比例，默认为1，即如果空间不足，该项目将缩小。
  - 负值无效
- flex-basis
  - 可以理解为content-box上width的值一样作用
  - 默认值为`auto`，即项目的本来大小
- flex
  - flex-grow,flex-shrink,flex-basis三个的简写
  - 默认值0 1 auto
  - 填auto，相当于（1 1 auto）
  - 填none，相当于（0 0 auto）
- align-self
  - 单个子项的对齐方式
  - 默认值auto，表示继承父元素的align-items属性，没有父元素，则等同stretch
  - auto,flex-start,flex-end,center,baseline,stretch

## Grid语法及使用

二维布局

### 容器

- display:grid，行内元素也可以设成display: inline-grid
- grid-template-columns
  - 定义每一列的列宽
  - 单位可以时绝对单位，百分数，fr
  - 属性值可以是repeat()函数
    - 此函数接受两个参数，第一个是重复次数（可以填auto-fil，表示自动填充），第二个是所要重复的值
  - minmax()，接受两个参数（最小值和最大轴），产生一个长度范围
  - 可以指定网格线名字
    - 例如：` [c1] 100px [c2] 100px [c3] auto [c4];`
- gird-template-rows
  - 定义每一行的行高
  - 单位可以时绝对单位，百分数，fr
  - repeat()
  - minmax()
  - 可以指定网格线名字
- grid-row-gap
  - 设置行与行的间隔
- gird-column-gap
  - 设置列与列的间隔
- grid-gap
  - 合并上面两个，先行后列
- grid-template-areas
  - 用于定义区域
  - 如果某些区域不需要利用，则使用"点"（`.`）表示
- grid-auto-flow
  - 划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格
  - 默认值row，先行后列；column，先列后行；`row dense`表示"先行后列"，并且尽可能紧密填满，尽量不出现空格；`column dense`表示"先列后行"，并且尽可能紧密填满，尽量不出现空格
- justify-items
  - 设置**单元格内容**的水平位置（左中右）
  - start,end,center,stretch(默认值)
- align-items
  - 设置**单元格内容**的垂直位置（上中下）
  - start,end,center,stretch(默认值)
- place-items
  - 合并简写形式
- justify-content
  - **整个内容区域**在容器里面的水平位置（左中右）
  - start,end,center,stretch,space-around,space-between,space-evenly(项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔)
- align-content
  - **整个内容区域**在容器里面的垂直位置（上中下）
  - 属性值一样
- place-content
  - 上面的合并形式
- grid-auto-columns
- grid-auto-rows
- grid-template
  - `grid-template-columns`、`grid-template-rows`和`grid-template-areas`这三个属性的合并简写形式
- grid
  - `grid-template-rows`、`grid-template-columns`、`grid-template-areas`、 `grid-auto-rows`、`grid-auto-columns`、`grid-auto-flow`这六个属性的合并简写形式

### 子项

- grid-column-start
- grid-column-end
- grid-row-start
- grid-row-end
  - 指定项目位置，属性值是第几条边框线
- grid-column
  - `grid-column-start`和`grid-column-end`的合并简写形式
  - 中间加`/`分隔开
- gird-row
  - `grid-row-start`属性和`grid-row-end`的合并简写形式
  - 中间加`/`
- grid-area
  - 指定项目放在哪一个区域，和容器属性值grid-template-area搭配
  - 还可用作`grid-row-start`、`grid-column-start`、`grid-row-end`、`grid-column-end`的合并简写形式
    - 中间加`/`
- justify-self
  - 跟`justify-items`属性的用法完全一致，但只作用于单个项目
- align-self
  - 跟`align-items`属性的用法完全一致，也是只作用于单个项目
- place-self
  - `align-self`属性和`justify-self`属性的合并简写形式

## CSS求值过程

![value](/images/CSS%E5%9F%BA%E7%A1%80/value.svg)

## 常问面经

### 隐藏元素方法

- display:none
  - 渲染树上直接移除，不占据任何空间
  - 产生回流和重绘
- visibility:hidden
  - 仍然在渲染树中，占据空间依然存在
  - 只产生重绘
  - 子元素设置visibility：visible，可以让子孙节点显示
- opacity:0
- position:absolute，将它移出屏幕
- z-index:负值
- transform:scale(0,0)
  - 元素缩小到0，元素仍在页面中占据位置，但是不会响应绑定的监听事件。

### 单位区别

- px为绝对像素
  - 我们写的是逻辑像素，实际测的是物理像素
  - 图片像素>物理像素，才不会失真
- em相对长度，是相对父元素
- rem相对长度，是相对根元素
- %，是相对父元素的百分之几
- vh、vw是相对视口
- vmin，vw和vh中较小值
- vmax，vw和vh中较大值

### 如何判断元素是否在可视区域

### 如何做适配

- 使用CSS媒体查询
- 使用相对单位

## 常用属性

### 颜色

- `#`
- `rgb()`
- `hsl()`
  - 色相
  - 饱和度
    - 0-100%
  - 亮度
    - 0-100%
- 关键字
- `rgba`
  - a是透明度，0是完全不透明
- `hsla`

### 字体

- `font-family`
  - 可以设定多个字体，用逗号隔开，从左到右逐个匹配，匹配到就使用，不再向后匹配
  
  - 字体列表最后要使用通用字体族
  
  - 英文字体要放在中文字体前面
    - 因为，中文一般包含了英文字体，中文字体放前面，一般都被匹配了，那么放在后面的英文字体就匹配不了英文字体
- Web font

  - 从网络加载资源
- `font-size`
  - 关键字
  - `px`
  - `em`、`rem`
  - `%`
- `font-weight`
  - 关键字
    - `normal`（400）
    - `bold`（700）
  - 直接设置数值
    - 有时候系统没有那么多对应数值的font-weight字体，所以，一般都是设成400或者700，以适应所有系统
- `line-height`
  - 行高
  - 数值带单位
  - 数值不带单位
    - 数值乘以font-size值
- `font-(style weight size/height family)`
- `text-align`
  - `left`
  - `right`
  - `center`
  - `justify`
    - 两端对齐
- `spacing`
  - `letter-spacing`
  - `word-spacing`
- `text-indent`
  - 文字缩进
  - 可正可负
- `text-decoration`
- `white-space`



`overflow`

- `visible`
  - 溢出
- `hidden`
- `scroll`
