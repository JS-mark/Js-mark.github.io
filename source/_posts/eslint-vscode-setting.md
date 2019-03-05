---
layout: post
title: 'eslint-vscode-setting'
subtitle: "eslint-vscode-setting"
date: 2018-11-30
author: 'Mark'
categories: JavaScript #分类
header-img: 'img/post-bg-js-version.jpg'
top: 2
tags:
  - 前端开发
  - JavaScript
  - VSCode
  - ESLint
---
> ## 第一步：全局安装 eslint,babel-eslint,eslint-plugin-html,eslint-plugin-react,eslint-plugin-vue

```bash
npm i eslint babel-eslint eslint-plugin-html eslint-plugin-react eslint-plugin-vue -g
```

> ## 第二步：在任意目录放置.eslintrc.js
>
> ## 第三步：在 vscode 下载 ESLint,Prettier - Code formatter,stylus,language-stylus,Vetur
>
> ## 第四步：在 vscode 中的配置

```javascript
	// eslint config start
	"eslint.autoFixOnSave": true,
	"eslint.options": {
		"configFile": "C:/Users/Mark/.eslint/.eslintrc.js"
	},
	"eslint.validate": [
		"javascript",
		"javascriptreact",
		"html",
		"vue",
		{
			"language": "vue",
			"autoFix": true
		}
	],
	"vetur.format.options.tabSize": 2,
	"vetur.format.options.useTabs": true,
	"vetur.format.defaultFormatterOptions": {
		"prettier": {
			// Prettier option here
			"semi": false,
			"tabWidth": 2,
			"useTabs": true,
			"singleQuote": true
		},
		"prettyhtml": {
			"printWidth": 100, // No line exceeds 100 characters
			"singleQuote": false // Prefer double quotes over single quotes
		}
	},
	// prettier 格式化配置
	"prettier.tabWidth": 2,
	"prettier.useTabs": true,
	"prettier.singleQuote": true,
	"prettier.semi": false,
	"stylusSupremacy.insertColons": false, // 是否插入冒号
	"stylusSupremacy.insertSemicolons": false, // 是否插入分好
	"stylusSupremacy.insertBraces": false, // 是否插入大括号
	"stylusSupremacy.insertNewLineAroundImports": false, // import之后是否换行
	"stylusSupremacy.insertNewLineAroundBlocks": false,
```
