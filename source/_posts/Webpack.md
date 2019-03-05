---
title: Webpack打包工具总结
date: 2017-12-29 01:01:16
categories: JavaScript
tags: [Webpack3.10, 语法, JS]
description: Webpack打包工具语法学习
top: 10
---

# Webpack

- 安装 webpack
- 配置 webpack.config.js
  > 官方教程：https://doc.webpack-china.org/configuration/#-

```javascript
var path = require('path');
module.exports = {
	entry: './foo.js',
		output: {
		path: path.resolve(__dirname, 'dist'),
		filename: 'foo.bundle.js'
	}
	module:
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: 'babel-loader'，
				include: [
		  path.resolve(__dirname, "app")
		],
		exclude: [
		  path.resolve(__dirname, "app/demo-files")
		],
		// 这里是匹配条件，每个选项都接收一个正则表达式或字符串
		// test 和 include 具有相同的作用，都是必须匹配选项
		// exclude 是必不匹配选项（优先于 test 和 include）
		// 最佳实践：
		// - 只在 test 和 文件名匹配 中使用正则表达式
		// - 在 include 和 exclude 中使用绝对路径数组
		// - 尽量避免 exclude，更倾向于使用 include
			}
		]
		plugins: [
		new (webpack.optimize.UglifyJsPlugin)
		new HtmlWebpackPlugin(template: './src/index.html')
	  ]
};
```

- 模块打包（默认只能打包 JS 模块，规则 CommonJS 等模块规范），让 webpack 支持其他文件类型打包，要选择合适的 loader - nodejs 书写模块规范 模块化规范 CommonJs,AMD,ES6 modules,
- Webpack - build-tool 构建工具 - loader webpack 默认只能打包 JS，loader 可以帮助我们打包其他的文件类型 - sass-loader 下载时，必须安装 ruby 或者 python 环境才能使用； - 安装 webpack-dev-server 热启动插件，必须在项目在安装 webpack，要不然会报错！ - webpack 使用方法：
  在命令行 输入 webpack 入口文件(app.js) 输出文件（build.js） - 配置 webpack ； 使用 webpack.config.js；让 webpack 支持其他文件类型打包，要选择合适的 loader - url-loader 和 file-loader 类似，url-loader 加载不了的使用 file-loader 加载； - HtmlWebpackPlugin 插件(自动在 output 目录中生成文件)以及，配置安装

```javascript
// npm install --save-dev html-webpack-plugin
// 在webpack.config.js中配置：
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
	entry: {
		app: './src/index.js',
		print: './src/print.js'
	},
	plugins: [
		new cleanWebpackPlugin(['dist']), //数组内可以放置多个要删除的目录，放置在HtmlWebpackPlugin插件前
		new HtmlWebpackPlugin({
			title: '页面标题', //生成页面标题
			filename: 'index.html', //要生成的文件名
			template: 'index.html' //要生成页面的时候的模板
		})
	],
	output: {
		filename: '[name].bundle.js',
		path: path.resolve(__dirname, 'dist')
	}
}
```

- JSon 文件中不能以有注释
- 使用 package.json 中的 scripts 键名是要启动的命令的简写，值是要启动的命令（这个个命令可以随意写，反正就是要在命令行中执行的命令，就可以写在这里）；

```javascript
  // 例:
  "scripts": {
    "dev": "webpack-dev-server --inline --hot --open --port 3000"
  },
  // 启动命令为 npm run dev
  // 例:
  "scripts": {
    "start": "webpack-dev-server --inline --hot --open --port 3000"
  },
  // 启动命令为 npm start
	// 如果键名是start，可以省略写run
```

- 配置 HMR 模块热替换，热替换这个插件，必须配置在项目目录，因为配置全局的话，不会有热替换的效果，浏览器不会自动刷新；插件 webpack-dev-sever 在 package.json

```javascript
- "scripts": {
	"start": "webpack-dev-server --inline --hot --open --port 3000"
	}
```

- 配置 ES6 语法降级，bable-loader，以及 bable-core,bable 依赖的核心库，bable-preset-env 语法字典库

```javascript
{
  test: /\.js$/,
  exclude: /(node_modules|bower_components)/,//忽略目录
  use: {
    loader: 'babel-loader',
    options: {
      presets: ['@babel/preset-env']
    }
  }
}
```

- 解析 vue 模板，vue-loader，这个模板安装后，可能会发生错误，就是需要在安装另外一个模块，安装上就好了！
- 解析文件的话，要去下载各种文件类型的 loader
- webpack 可以打包各种模块，js 就是模块或者说是包，我们可以直接使用 CommenJS 或者 ES6 等规范的语法，导入各种各样我们需要的模块，并把它并把导入的模块用对象包裹起来，我们就可以调用里边的方法了
- package.json 对象中最后一个参数项，不能书写逗号

### CLI

- （command-line interface，命令行界面）是指可在用户提示符下键入可执行指令的界面，它通常不支持鼠标，用户通过键盘输入指令，计算机接收到指令后，予以执行。CLI 在汇编指令中也有关闭中断的意思
- vue-cli vue 脚手架 ，是为了快速构建一个项目环境的命令行操作工具

### 打包的工程目录中 src 源码所在文件，dist 发布的目录
