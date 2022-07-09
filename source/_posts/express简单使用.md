---
title: express简单使用
date: 2022-03-31 14:44:45
tags: express
categories: node
---

## 安装

1. 脚手架
   - `npm install -g express-generator`
   - `express express-demo`
   - `npm install`
   - `node bin/www`
2. 从零搭建
   - `npm init -y`
   - `npm install express`

<!--more-->

## 基本使用

```javascript
const express = require('express');

// express其实是一个函数: createApplication
// 返回app
const app = express();

// 监听路径
app.get('/', (req, res, next) => {
  res.end("Hello Express");
})

// 开启监听
app.listen(8000, () => {
  console.log("express初体验服务器启动成功~");
});

```

## 中间件

> 中间件函数能够访问请求对象 (`req`)、响应对象 (`res`) 以及应用程序的请求/响应循环中的下一个中间件函数

中间件函数可以执行以下任务：

- 执行任何代码。
- 对请求和响应对象进行更改。
- 结束请求/响应循环。
- 调用堆栈中的下一个中间件函数。

如果当前中间件函数没有结束请求/响应循环，那么它必须调用 `next()`，以将控制权传递给下一个中间件函数。否则，请求将保持挂起状态。

### 普通中间件

```javascript
// use注册一个中间件(回调函数)
app.use((req, res, next) => {
  console.log("注册了一个普通的中间件~");
  next();
});
```

### 路径中间件

```javascript
// 路径匹配的中间件
app.use('/home', (req, res, next) => {
  console.log("home middleware 01");
});
```

### 路径和方法中间件

```javascript
app.get('/home', (req, res, next) => {
  console.log("home path and method middleware01");
});
```

### 连续注册中间件

```javascript
app.get("/home", (req, res, next) => {
  console.log("home path and method middleware 02");
  next();
}, (req, res, next) => {
  console.log("home path and method middleware 03");
  next();
}, (req, res, next) => {
  console.log("home path and method middleware 04");
  res.end("home page");
});
```

## 应用中间件 body解析

### 手动解析

```javascript
// 手动解析
app.use((req, res, next) => {
  if (req.headers["content-type"] === 'application/json') {
    req.on('data', (data) => {
      const info = JSON.parse(data.toString());
      req.body = info;
    })
  
    req.on('end', () => {
      next();
    })
  } else {
    next();
  }
})
```

### 内置解析

```javascript
// express内置解析
app.use(express.json())
```

### urlencoded解析

```javascript
// extended
// true: 那么对urlencoded进行解析时, 它使用的是第三方库: qs
// false: 那么对urlencoded进行解析时, 它使用的是Node内置模块: querystring
app.use(express.urlencoded({extended: true}));
```

### 上传文件

```javascript
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, './uploads/');		//把文件放置的位置
  },
  filename: (req, file, cb) => {
    // 文件命名
    cb(null, Date.now() + path.extname(file.originalname));
  }
})

const upload = multer({
  // dest: './uploads/'
  storage
});

app.post('/login', upload.any(), (req, res, next) => {
  console.log(req.body);
  res.end("用户登录成功~")
});
```

### form-data解析

```javascript
const upload = multer();
app.use(upload.any());
```

## 查询参数params和query

```javascript
req.params

req.query
```

## 响应数据

- end
- json
  - 可以传入很多的类型：object、array、string、boolean、number、null等，它们会被转换成json格式返回
- status

## express路由

使用 `express.Router` 类来创建可安装的模块化路由处理程序。`Router` 实例是完整的中间件和路由系统；因此，常常将其称为“微型应用程序”

```javascript
// 在userRoute.js文件中，编写关于user的中间件
const express = require('express');
// 创建可安装的模块化路由处理程序
const router = express.Router();
router.get('/:id', (req, res, next) => {
  res.json(`${req.params.id}用户的信息`);
});
module.exports = router;
```

```javascript
// 在app.js文件中引入user路由
const userRouter = require('./routers/users');
const app = express();

// 这里指定路径，userRoute.js中就可以省略/user
app.use("/users", userRouter);
```

## 静态服务器

```javascript
app.use(express.static('./build'));
```

