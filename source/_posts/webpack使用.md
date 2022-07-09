---
title: webpack使用
date: 2022-07-09 11:20:25
tags: webpack
---

## Webpack

> 静态模块化打包工具
>
> 打包流程
>
> 1. 根据命令或配置文件找到入口文件
> 2. 从入口开始，生成一个依赖关系图，此依赖关系图包含程序**所需**的所有模块
> 3. 根据依赖关系图，遍历图结构，打包一个个模块

- 入口
- 出口
- loader
- plugin

<!--more-->

## 安装Webpack

1. 创建package.json文件，用于管理项目信息、库依赖
2. 安装局部webpack
3. 使用webpack

```shell
npm init
npm install webpack -D
npm install webpack-cli -D #webpack4.0+执行时需要依赖到webpack-cli
npx webpack #npx会去当前根目录的node_module里找webpack，不会去全局找
```

4. 可以在package.json文件中，添加脚本（）

```shell
  "scripts": {
    "build": "webpack",
  }
  # 之后运行npm run build就可执行脚本打包，且webpack使用的使局部
```

5. 根目录`webpack.config.js`文件进行配置

```javascript
const path = require('path');
module.exports = {
  entry: "./src/index.js", //入口文件
  output: {
    filename: "bundle.js",	//出口文件
    path: path.resolve(__dirname, "./dist")	//出口文件夹
  },
}
```

## loader 的简单使用

> *loader* 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

## 样式

根目录下新建文件

- src
  - css
    - style.css
  - js
    - element.js

```javascript
// elememt.js
import "../css/style.css"

const divEl = document.createElement('div');
divEl.className = "title";
divEl.innerHTML = "hello";
```

```css
/* style.css */
.title {
  background-color: skyblue;
}
```

执行`npm run build`会直接报错，因为缺少解析style.css的loader

所以需要安装`css-loader`

```shell
npm install css-loader -D
```

在`webpack.config.js`配置`css-loader`

```javascript
//webpack.config.js
module: {
    rules: [
        {
            test: /\.css$/,	//对资源进行匹配
            use: [
                "css-loader"
            ]
        },
    ]
}
```

再次运行`npm run build`没报错，但css没生效，因为css-loader只会进行解析，不会将解析后的css插入页面中

需要安装`style-loader`

```shell
npm install style-loader -D
```

同样在`webpack.config.js`配置`style-loader`

```javascript
//webpack.config.js
module: {
    rules: [
        {
            test: /\.css$/,
            use: [
                "style-loader"
                "css-loader"	// loader会用顺序，先解析的放后面
            ]
        },
    ]
}
```

运行`npm run build`页面会成功展示css

同样道理可以配置解析`less`，`scss`

安装`less-loader`，并进行配置可以就less文件转换为css

```shell
npm install less-loader -D
```

```javascript
//webpack.config.js
module: {
    rules: [
        {
        test: /\.(png|jpe?g|gif|svg)$/i,
        type: "asset"
        // use: {
        //   loader: "file-loader",
        //   options: {
        //     outputPath: "img",
        //     name: "[name]_[hash:8].[ext]"
        //   }
        // }
        },
    ]
}
```



## 图片

安装`file-loader`

```javascript
//webpack.config.js
module: {
    rules: [
        {
            test: /\.(png|jpe?g|gif|svg)$/i,
            use: {
              loader: "file-loader",
            }
        },
    ]
}
```

打包后的文件名会是乱码，如果要保留名字，同时防止命名冲突，可以进行配置

```javascript
//webpack.config.js
module: {
    rules: [
        {
            test: /\.(png|jpe?g|gif|svg)$/i,
            use: {
              loader: "file-loader",
              options: {
                outputPath: "img",	// outputPath来设置输出的文件夹
                name: "[name]_[hash:8].[ext]"
              }
            }
        },
    ]
}
```

- [ext]： 处理文件的扩展名
- [name]：处理文件的名称
- [hash]：文件的内容，使用MD4的散列函数处理，生成的一个128位的hash值（32个十六进制）
- [hash:<length>]：截图hash的长度

## Plugin 的简单使用

> loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

### CleanWebpackPlugin

打包后，自动删除旧的包

1. 安装插件

```shell
npm install clean-webpack-plugi -D
```

2. 配置插件

```javascript
const { CleanWebpackPlugin } = require("clean-webpack-plugin")
module.exports = {
  plugins: [
    new CleanWebpackPlugin()
  ]
}
```

### HtmlWebpackPlugin

因为打包后是没有index.html文件，而进行项目部署必须要对应入口文件`index.html`，所以也要对`index.heml`进行打包

1. 安装插件

```shell
npm install html-webpack-plugin -D
```

2. 配置插件

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      title: "webpack案例",
      template: "./src/public/index.html"	// 可以自己添加模板，不用模板，会自动生成最基本的index.html文件
    }),
  ]
}
```

### DefinePlugin

此插件是webpack内置的，用于允许在编译时创建配置的全局常量

```javascript
const { DefinePlugin } = require('webpack');
module.exports = {
  plugins: [
    new DefinePlugin({
      BASE_URL: '"./"',
      __VUE_OPTIONS_API__: true,
      __VUE_PROD_DEVTOOLS__: false
    })
  ]
}
```

### CopyWebpackPlugin

可以将需要的文件，复制到dist文件夹中

1. 安装

```shell
npm install copy-webpack-plugin -D
```

2. 配置

```javascript
const CopyWebpackPlugin = require("copy-webpack-plugin")
module.exports = {
  plugins: [
    new CopyWebpackPlugin({
      patterns: [
        {	// to: 设置复制到的位置，默认复制到打包的目录下
          from: "./src/public",	// 设置从哪一个源开始复制
          globOptions: {
            ignore: [	// 需要忽略的文件
              '**/index.html'
            ]
          }
        }
      ]
    }),
  ]
}
```

## Babel

> 解决语法转换、源代码转换。主要用于旧浏览器或者环境中将ECMAScript 2015+代码转换为向后兼容版本的 JavaScript

1. 安装

```shell
npm install @babel/cli #babel的核心代码
npm install @babel/plugin-transfor-arrow-functions -D # 箭头函数转换的插件
npm install @babel/plugin-transform-block-scoping -D # 用于const，let转为var
```

2. 使用

```shell
npx babel src --out-dir dist --plugins=@babel/plugin-transform-block-scoping,@babel/plugin-transform-arrow-functions
```

- src：是源文件的目录
- --out-dir：指定要输出的文件夹dist

### 预设preset

1. 安装

```shell
npm install @babel/preset-env -D
```

2. 使用

```shell
npx babel src --out-dir dist --presets=@babel/preset-env
```

### babel-loader

搭配在webpack中使用

1. 安装依赖

```shell
npm install babel-loader @babel/core
```

2. 配置

```javascript
     
```

```javascript
//webpack.config.js
module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: "babel-loader",
          options: {
             plugins: [	//指定使用的插件
               "@babel/plugin-transform-arrow-functions",
               "@babel/plugin-transform-block-scoping"
             ]
          }
        }
      }
    ]
}
```

如果一个个配置babel的插件显然不合理，我们可以使用babel-preset，webpack会根据我们的预设来加载对应的插件列表，并且将其传递给babel。

1. 安装

```shell
npm install @babel/preset-env
```

2. 配置

```javascript
//webpack.config.js
module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: "babel-loader",
          options: {
            presets: [
              "@babel/preset-env"
            ]
          }
        }
      }
    ]
}
```

babel可以放在独立的配置文件

在根目录新建`.babel.config.json`

```javascript
module.exports = {
  presets: [
    ["@babel/preset-env"]
  ]
}
```

## vue-loader

> 用来处理.vue文件

1. 安装

```shell
npm install vue-loader -D
```

2. 配置

```javascript
      {
        test: /\.vue$/,
        loader: "vue-loader"
      }
```

3. 还需要添加@vue/compiler-sfc来对template进行解析

```shell
npm install @vue/compiler-sfc -D
```

```javascript
const { VueLoaderPlugin } =require("vue-loader/dist/index");
plugins: [
    new VueLoaderPlugin()
  ]
```

## 本地服务器

> 当文件发生变化时，可以自动的完成编译 和展示

### webpack watch mode

- 可以在`webpack.config.js`中加入`watch:true`
- 或者在`package.json`文件中加入脚本

```shell
  "scripts": {
    "watch": "webpack --watch",
  }
```

### webpack-dev-serve

> 用于实时重新加载

1. 安装

```shell
npm install webpack-dev-serve -D
```

2. 配置

```javascript
  devServer: {
    hot: true,
  },
```

## Proxy配置选项可以解决跨域访问问题

请求先发送到一个代理服务器，代理服务器和API服务器没有跨域的问题，就可以解决我们的跨域问题了

设置选项

- target：表示的是代理到的目标地址，比如 /api/moment会被代理到 http://localhost:8888/api/moment
- pathRewrite：默认情况下，我们的 /api 也会被写入到URL中，如果希望删除，可以使用pathRewrite
- secure：默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false

## resolve模块解析

```javascript
  resolve: {
    extensions: [".js", ".json", ".jsx", ".ts", ".vue"],	//自动添加扩展名
    alias: {	// 起别名
      "@": path.resolve(__dirname, "./src")
    }
  },
```

