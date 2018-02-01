---
layout: post
title: webpack构建工具
category: default
tags: webpack
---


#### 简介
Webpack 是一个开源的前端打包工具。
将各种静态资源视为模块，并从它生成优化过的代码。

---

# ![webpack](https://webpack.github.io/assets/what-is-webpack.png)


<!-- more -->

---

#### webpack和Grunt/Gulp的区别
- webpack 是一个模块打包器(module bundler)，打包器是帮助你取得准备用于部署的 JavaScript 和样式表，将它们转换为适合浏览器的可用格式。
- Grunt/Gulp是一个任务执行器(task runner)，任务执行器是用来自动化处理常见的开发任务，例如项目的检查(lint)、构建(build)、测试(test)。

---

#### 概念
- 入口(entry)
- 输出(output)
- loader
- 插件(plugins)

---
#### 使用Loader的几种方式
- 配置（推荐）：在 webpack.config.js 文件中指定 loader。
```module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' }
        ]
      }
    ]
  }

```
- 内联：在每个 import 语句中显式指定 loader。
```
import Styles from 'style-loader!css-loader?modules!./styles.css';
```
- CLI：在 shell 命令中指定它们。
```
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

---

##### 版本
v3.10.0

##### 安装
```
yarn add webpack //yarn
npm install --save-dev webpack //npm安装最新版本
npm install --save-dev webpack@<version> //npm安装特定版本
npm install webpack@beta //安装beta版本
```

---

##### 单入口文件
```
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      }
    ]
  }
};

```

---

##### 多入口文件
```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  entry: {
    app: './src/index.js',
    print: './src/print.js'
  },
  plugins: [
    new CleanWebpackPlugin(['dist']),
    new HtmlWebpackPlugin({
      title: 'Output Management'
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};

```

---

#### 插件

- html-webpack-plugin  //打包html
- clean-webpack-plugin //清空文件


#### Devtool
https://doc.webpack-china.org/configuration/devtool

---

#### 热更新

1. 使用观察者模式
package.json
```
"scripts": {
    "watch": "webpack --watch"
 }
```
---

2. 使用 webpack-dev-server
提供一个简单的 web 服务器，并且能够实时重新加载(live reloading)
```
 devServer: {
   contentBase: './dist'
 },
```
以上配置告知 webpack-dev-server，在 localhost:8080 下建立服务，将 dist 目录下的文件，作为可访问文件。
```
 "start": "webpack-dev-server --open",
 ```

---

3. 使用 webpack-dev-middleware

webpack-dev-middleware 是一个中间件容器(wrapper)，它将通过 webpack 处理后的文件发布到一个服务器(server)。webpack-dev-server内部使用 webpack-dev-middleware，然而，它可以作为一个单独的包来提供，可以进行更多的自定义设置来实现更多需求。webpack-dev-middleware 配合 express server
```
server": "node server.js"
```

---

4. 启用HMR
```
devServer: {
   contentBase: './dist',
   hot: true
}

new webpack.HotModuleReplacementPlugin()
```
---

#### Tree Shaking

Tree Shaking是移除 JavaScript 上下文中的未引用代码(dead-code)
webpack build时会标识出那些“未引用代码(dead code)”，但不会删除它们。
实现删除，添加压缩工具(minifier) - UglifyJSPlugin
```
yarn add uglifyjs-webpack-plugin
或
npm install --save-dev uglifyjs-webpack-plugin
```
---

#### 生产环境构建
- 开发环境中，我们需要实时重新加载(live reloading)或热模块替换(hot module replacement)能力、source map 和 localhost server。
- 生产环境，目标是更小的 bundle，更轻量的 source map

- webpack-merge --保留一个“通用”配置
```
webpack.common.js
webpack.dev.js
webpack.prod.js
```
```
 "start": "webpack-dev-server --open --config webpack.dev.js",
 "server": "node server.js",
 "build": "webpack --config webpack.prod.js"
```
---

#### 代码分离
有三种常用的代码分离方法：

- 入口起点：使用 entry 配置手动地分离代码。
- 防止重复：使用 CommonsChunkPlugin 去重和分离 chunk。
- 动态导入：通过模块的内联函数调用来分离代码。

---

#### 缓存
- 通过使用 output.filename 进行文件名替换
```
filename: '[name].[chunkhash].js',
```
- 提取模板

---

#### 构建性能

- 保持版本最新
- Loaders
将loaders 应用于最少数的必要模块中
```
include: path.resolve(__dirname, "src")
```


