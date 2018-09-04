---
title: react-baseui
date: 
tags: Projects
categories: Projects
---
基于react实现的baseui,功能不尽完善,重在学习参与。

<!-- more -->

### 快速上手
react-baseui 致力于提供给程序员**愉悦**的开发体验。
在开始之前，推荐先学习 [React][1] 和 [ES2015(ES6][2])。


### 标准开发

实际项目开发中，你会需要对 `ES2015` 和 `JSX` 代码的构建、调试、代理、打包部署等一系列工程化的需求。 我们提供了一套` npm` + `webpack` 的开发工具链来辅助开发，下面我们用一个简单的实例来说明。

#### 1. 安装脚手架工具

使用前，务必确认 `Node.js` 已经升级到 v4.x 或以上。
``` javascript
$ npm install -g
```
更多功能请参考 脚手架工具 和 开发工具文档。
除了官方提供的脚手架，您也可以使用社区提供的脚手架和范例


#### 2. 创建一个项目

使用命令行进行初始化。

    $ mkdir demo && cd demo
    $ git init

`npm install`会自动安装 npm 依赖，若有问题则可自行安装。
若安装缓慢报错，可尝试用 `cnpm` 或别的镜像源自行安装：`rm -rf node_modules && cnpm install`。 

#### 3. 使用组件

用 React 的方式直接使用 组件。

```javascript
import ReactUIBase from '../framework/uibase'
import React from 'react'
import HorizontalLayout from './HorizontalLayout'
import FontIcon from './FontIcon'
import Text from './Text'

class Radio extends ReactUIBase {
	static get propTypes() {
		return {
			onSelect: React.PropTypes.func,
			values: React.PropTypes.arrayOf(React.PropTypes.string),
			default: React.PropTypes.number,
		}
	}
	static get defaultProps() {
		return {
			values: ['请不要', '玩我'],
			onSelect: index => index,
			default: -1,
		}
	}

	constructor(props) {
		super(props);
		this.state = {
			select: props.default
		};
	}

	itemClick(index) {
		this.setState({
			select: index
		});
		this.props.onSelect(index);
	}

	render() {
		if (this.props.values.length == 0) {
			return <Text>搞什么飞机3!!!</Text>
		}

		var items = this.props.values.map((item, index) => {
			var icon = index == this.state.select ? 'icon-radio-yes' : 'icon-radio-none';
			return (
				<HorizontalLayout onClick={this.itemClick.bind(this,index)} key={index}>
				<FontIcon icon={icon} /><Text>{item}</Text>
				</HorizontalLayout>
			)
		});

		return (
			<HorizontalLayout>
			{items}
			</HorizontalLayout>
		)
	}
}


module.exports = Radio;
```

> 你可以在左侧菜单选用更多组件。

#### 4. 开发调试

一键启动调试，访问http://127.0.0.1:9303 查看效果。

    $ node server

#### 5. 构建和部署

    $ npm run build

入口文件会构建到 `dist` 目录中，你可以自由部署到不同环境中进行引用。

> 上述例子用于帮助你理解 react-baseui 的使用流程，并非真实的开发过程，你可以根据自己的项目开发流程进行接入。

### 兼容性

react-baseui 支持所有的现代浏览器和 IE9+。
对于 IE8/9 等浏览器，需要提供 `es5-shim` 等 `Polyfills` 的支持，推荐使用 `babel-polyfill`
```javascript
<!doctype html public="storage">
<html>
<head>
<meta charset=utf-8/>
<title><%= title %></title>
 <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
 <meta name="format-detection" content="telephone=no" />
<script type="text/javascript"  src="http://cdn.bootcss.com/mathjax/2.6.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML&delayStartupUntil=configured"></script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [["$", "$"]]
    },
    "HTML-CSS": {linebreaks: {automatic: true}, matchFontHeight: false},
    SVG: {linebreaks: {automatic: true}, matchFontHeight: false}
});
</script>   
	<script src="/3party/superagent.min.js"></script>
	<script src="/3party/jquery-1.12.4.min.js"></script>
	<script src="/3party/react.min.js"></script>
	<script src="/3party/react-dom.min.js"></script>
	<script src="/3party/redux.min.js"></script>
	<script src="/3party/react-redux.min.js"></script>
	<script src="/3party/redux-thunk.min.js"></script>
	<script src="/3party/underscore-min.js"></script>
	<script src="/3party/ReactRouter.min.js"></script>
	<script src="/3party/redux-logger.min.js"></script>
	<script src="/3party/polyfill.min.js"></script>
	<script src="/3party/browser.min.js"></script>
	<script src="/3party/moment.min.js"></script>
	<script src="/3party/echarts.min.js"></script>
	<script src="/3party/md5.min.js"></script>
	<script src="/imsdk.js"></script>
	<script src="/yxbaseui.min.js"></script>
	<script src="/yxframework.js"></script>
	<script src="/myrequire.js"></script>
	<script type="text/babel" src="/framework/app.js"></script>
<style type="text/css">
    body{
		padding:0; 
		margin:0;
		display: flex;
		flex-direction: column;
		height: 100vh;
		background-color: #ebecee;
		font-family: Microsoft YaHei,'微软雅黑',Arial,Tahoma,STHeiti,Helvetica,"\5b8b\4f53",sans-serif;
	}
		.inOutRight-enter {
		  position:absolute;
		  right:-700;
		  bottom:0;
		}

		.inOutRight-enter.inOutRight-enter-active {
  		  position:absolute;
  		  right:0;
		  bottom:0;
		  transition: right 300ms ease-in;
		}

		.inOutRight-leave {
  		  position:absolute;
		  bottom:0;
  		  right:0;
		}

		.inOutRight-leave.inOutRight-leave-active {
    	  position:absolute;
		  bottom:0;
		  right:-700;
		  transition: right 200ms ease-in;
		}
</style>

<link rel="stylesheet" href="/ello.css">
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "//hm.baidu.com/hm.js?eac03860e2f759376017fff335b8ae62";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>

</head>
<body>
<div id="root" style='display:flex;flex-direction: column; height: 100vh;'><%- html %></div>

<script type="text/javascript" charset="utf-8">
  window.__REDUX_STATE__ = '<%= reduxState %>';
  window.__COMPS__ = '<%= comps %>';
  window.__RUNTIME__ = '<%= runtimeData %>';
</script>
<script type="text/babel" src="/componentmap.js"></script>
<script type="text/javascript" src="/browsercheck.js"></script>
</body>
</html>

```

> 更多 IE8 下使用 React 的相关问题可以参考：https://github.com/xcatliu/react-ie8

### 自行构建

如果想自己维护工作流，我们推荐使用 [webpack][4] 进行构建和调试。理论上你可以利用 React 生态圈中的 各种脚手架 进行开发，如果遇到问题可参考我们所使用的 [webpack 配置][5] 进行 [定制][6]。

### 按需加载

通过 `import { Button } from '../framework/uibase';` 引入会加载 下所有的模块，如果要按需加载可以通过以下的写法来引用。

    import Button from '../framework/uibase/button';


### 配置案例

- [改变主色系][7]
- [使用本地字体][8]

### 小甜点

- 你可以享用 `npm` 生态圈里的所有模块。
- 我们使用了 `babel`，试试用 `ES2015(ES6)` 的写法来提升编码的愉悦感。
- 参考并致谢 [ant-design](https://ant.design/docs/react/introduce) 。
- 项目地址 [react-baseui](https://github.com/sumodream/react-baseui) 。


  [1]: https://facebook.github.io/react/
  [2]: http://babeljs.io/docs/learn-es2015/
  [3]: http://codepen.io/anon/pen/wGOWGW?editors=001
  [4]: http://webpack.github.io/
  [5]: http://ant-tool.github.io/webpack-config.html
  [6]: https://github.com/enaqx/awesome-react#boilerplates
  [7]: https://github.com/ant-design/antd-init/tree/master/examples/customize-antd-theme
  [8]: https://github.com/ant-design/antd-init/tree/master/examples/local-iconfont