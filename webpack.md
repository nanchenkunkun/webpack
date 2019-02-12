#webpack
##快速了解几个基本的概念
### mode开发模式
webpack提供了mode配置选项，配置webpack相应模式的内置优化

		// webpack.production.config.js
		module.exports = {
			+  mode: 'production',
		}

###入口文件

入口文件，类似于其他语言的起始文件。比如：c 语言的 main 函数所在的文件。
入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

可以在 webpack 的配置文件中配置入口，配置节点为： entry,当然可以配置一个入口，也可以配置多个。

###输出(output)
output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。

		const path = require('path');
		
		module.exports = {
		  entry: './path/to/my/entry/file.js',
		  output: {
		    path: path.resolve(__dirname, 'dist'),
		    filename: 'my-first-webpack.bundle.js'
		  }
		};

###loader
loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理

###插件(plugins)
loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

#webpack 的安装

##本地安装webpack

		$ npm install --save-dev webpack
		
		# 如果你使用 webpack 4+ 版本，你还需要安装 CLI。
		npm install --save-dev webpack-cli
安装完成后，可以添加npm的script脚本

		// package.json
		"scripts": {
		    "start": "webpack --config webpack.config.js"
		}

#快速入门完整的demo
第一步：创建项目结构
首先我们创建一个目录，初始化 npm，然后 在本地安装 webpack，接着安装 webpack-cli（此工具用于在命令行中运行 webpack）：

	mkdir webpack-demo && cd webpack-demo
	npm init -y
	npm install webpack webpack-cli --save-dev

目录结构

- webpack-demo
	- package.json
	- /dist
		- index.html
	- /src
		- index.js	


第二步：安装 loadash 依赖和编写 js 文件
		
		npm isntall --save-dev lodash

编写：src/index.js 文件

		import _ from 'lodash';
		
		function createDomElement() {
		  var dom = document.createElement('div');
		  dom.innerHTML = _.join(['aicoder', '.com', ' wow'], '');
		  return dom;
		}
		
		document.body.appendChild(createDomElement());

index.html

		<!DOCTYPE html>
		<html lang="en">
		<head>
		  <meta charset="UTF-8">
		  <meta name="viewport" content="width=device-width, initial-scale=1.0">
		  <meta http-equiv="X-UA-Compatible" content="ie=edge">
		  <title>起步</title>
		</head>
		<body>
		  <script src="./main.js"></script>
		</body>
		</html>

第三步：编写 webpack 配置文件

根目录下添加 webpack.config.js文件。
webpack.config.js 内容如下：


		const path = require('path');
		
		module.exports = {
		  mode: 'development',
		  entry: './src/index.js',
		  output: {
		    filename: 'main.js',
		    path: path.resolve(__dirname, './dist')
		  }
		};

执行构建任务

		npm run start


#加载非 js 文件
##加载 CSS 文件
第一步： 安装 css 和 style 模块解析的依赖 style-loader 和 css-loader

		npm install --save-dev style-loader css-loader

第二步： 添加 css 解析的 loader

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
		        use: ['style-loader', 'css-loader']
		      }
		    ]
		  }
		};

css-loader： 辅助解析 js 中的 import './main.css'
style-loader: 把 js 中引入的 css 内容 注入到 html 标签中，并添加 style 标签.依赖 css-loader

第三步： 编写 css 文件和修改 js 文件
在 src 目录中添加 style.css文件
		 webpack-demo
		  |- package.json
		  |- webpack.config.js
		  |- /dist
		    |- bundle.js
		    |- index.html
		  |- /src
		+   |- style.css
		    |- index.js
		  |- /node_modules

src/style.css

		.hello {
		  color: red;
		}

修改 js 文件


		  import _ from 'lodash';
		+ import './style.css';
		
		  function createDomElement() {
		    let dom = document.createElement('div');
		    dom.innerHTML = _.join(['aicoder', '.com', ' wow'], '');
		+   dom.className = 'hello';
		    return dom;
		  }
		
		  document.body.appendChild(createDomElement());


最后重新打开 dist 目录下的 index.html 看一下文字是否变成了红色的了。