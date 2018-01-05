需要掌握的
webpack 热部署等webpack相关知识

webpack 热部署原理

文件修改保存后会触发 webpack  重新构建， 服务器向浏览器发送更新消息，浏览器通过jsonp请求更新模块，jsonp 回调触发模块热替换逻辑

模块热替换原理：
模块热替换(HMR - Hot Module Replacement)功能会在应用程序运行过程中替换、添加或删除模块，而无需重新加载整个页面。主要是通过以下几种方式，来显著加快开发速度：
* 保留在完全重新加载页面时丢失的应用程序状态。
* 只更新变更内容，以节省宝贵的开发时间。
* 调整样式更加快速 - 几乎相当于在浏览器调试器中更改样式。


http 网络请求
http 的发展史
http （超文本传输协议） 是浏览器和web服务器之间进行通信的一种协议
http 协议最常用的请求方法（GET,  POST，HEAD,  PUT,  DELETE）
GET 是最早请求的方法，指定资源请求数据，所有的参数在URL上，因为查询参数都是在URL上会有以下几个问题
（1）各个浏览器对 URL 长度限制问题，注定不能向服务器传递过多的参数
（2）所有参数都在URL的查询参数上，请求一目了然，安全性不高， 最容易造成csrf攻击
（3）适用于向服务器索取资源但不适合提交数据
所以有了POST请求的出现，post 请求规避了get请求的以上弊端， 当然post请求也不能完全规避，比如csrf攻击问题

head方法不是返回资源内容， 而是资源信息，以此来了解资源的更新状况

PUT 更新资源信息， 与POST相同的是参数都是在请求体中，与POST不同的是，POST是获取请求体然后对请求体进行分析，最后给客户端回传相应的处理结果，而PUT直接将请求体替换为URL的内容


DELETE 删除URL 指定的信息

chrome 会有请求头，请求体等， 需要了解一下整个http请求的各项参数

重绘和回流的区别

fetch 原理  ajax原理

接触的模板引擎（HTML）比如nunjunct


div 垂直居中方案 主要考察flex 布局

理解yield

站点性能优化

react  虚拟dom 结构， 

了解模式开发

http2.0

http restful 4种状态

session  持久化

虚拟dom的机制， diff原理



怎么看待redux, redux 的产生原因，

css bfc概念

localStorage 和 sessionStorage 这些h5的存储是需要了解的

箭头函数和普通函数不一样的点：
箭头函数数lanmada表达式，首先从lanmada表达式的优点上就可以看出，普通函数比箭头函数更加臃肿
普通函数： 可以作为构造函数，可以对普通函数做new操作，但是箭头函数不能
箭头函数没有arguments对象，取而代之是rest
var C = (...c)=>{ //...c即为rest参数
  console.log(c); //[3]
}

箭头函数没有原型属性， 也就是说箭头函数没有prototype 属性

实际上箭头函数中并不只是 this 和普通函数有所不同，箭头函数中没有任何像 this 这样自动绑定的局部变量，包括：this，arguments，super(ES6)，new.target(ES6)……


一步步在掌握的
DSL
领域专用语言， 在前端最直接的表现就是模板语言

模拟网络请求：chrome提供慢网络环境配置

nodejs 
npm生态， npm是node包的分发和管理工具， 有了npm，可以很快的找到特定服务要使用的包，进行下载、安装以及管理已经安装的包。

异步流程控制的演进：
callback hell (回调地狱) 怎么看待， nodejs 异步导致api设计方式是callback，但也因为回调地狱使得node发展出了thunk、promise，es6出现了generate,co等，es7出现了async wait,

callback	Node.js API天生就是这样的	
thunk	参数的求值策略	
promise	最开始是Promise/A+规范，随后成为ES6标准	
generator	ES6种的生成器，用于计算，但tj想用做流程控制	
co	generator用起来非常麻烦，故而tj写了co这个generator生成器，用法更简单	
async函数	原本计划进入es7规范，结果差一点，但好在v8实现了，所以node 7就可以使用，无须等es7规范落地

求值策略优势劣势
传值调用 先求值再传参：
优势：简单，好明白
劣势：还没用就计算，会有性能问题
传名调用 优劣式正好相反

http://www.ruanyifeng.com/blog/2015/05/thunk.html
thunk 函数其实是“传名调用”的一种实现策略,也是函数式编程的一种理念, npm 中有关于thunk 实现的库， 这个库就是thunkify， thunkify只允许callback调用一次


generate 的流程管理

你可能会问， Thunk 函数有什么用？回答是以前确实没什么用，但是 ES6 有了 Generator 函数，Thunk 函数现在可以用于 Generator 函数的自动流程管理。

var fs = require('fs');
var thunkify = require('thunkify');
var readFile = thunkify(fs.readFile);

var gen = function* (){
  var r1 = yield readFile('/etc/fstab');
  console.log(r1.toString());
  var r2 = yield readFile('/etc/shells');
  console.log(r2.toString());
};

上面代码中，yield 命令用于将程序的执行权移出 Generator 函数，那么就需要一种方法，将执行权再交还给 Generator 函数

这种方法就是 Thunk 函数，因为它可以在回调函数里，将执行权交还给 Generator 函数。为了便于理解，我们先看如何手动执行上面这个 Generator 函数。

var g = gen();

var r1 = g.next();
r1.value(function(err, data){
  if (err) throw err;
  var r2 = g.next(data);
  r2.value(function(err, data){
    if (err) throw err;
    g.next(data);
  });
});

node 应用场景：
网站： nodejs搭建框架： express koa
im即时聊天： socket.io
前端构建工具： gulp,webpack, grunt
工程化工具： proxy 代理
写操作： nodeOS
命令行工具： （比如cordova、shell.js
反向代理（比如anyproxy，node-http-proxy）
编辑器Atom、VSCode等

Express 和 koa 的区别
Express， 简单实用，路由，中间件，五脏俱全， 是比全面具体的nodejs应用web框架
koa 专注于异步流程控制的改进, 是下一代web框架


http 相关问题
http常用状态码：
200： 正常  表示一切正常, 返回的是正常请求结果
302/307：临时重定向　　指出请求的文档已被临时移动到别处, 此文档的新的url在location响应头中给出
304:  未修改(缓存)　　表示客户机缓存的版本是最新的, 客户机应该继续使用它
404：找不到　　服务器上不存在客户机所请求的资源
500　　服务器内部错误

cookie 也分几类：
不同维度有不同分类
维度一
（1）会话cookie(临时cookie): 存在内存中，浏览器关闭，cookie便会失效
（2）永久cookie: 存在硬盘中， 浏览器关闭后再打开，cookie 还是存在，只有设定的过期时间到了，cookie才会失效
维度二

（1）http-only cookie: 这种cookie在document不能被读取，也不能被修改， 只有服务端可以注入
（2）http： 这种cookie在客户端可读写。

request.cookie:是将内容从客户端浏览器里面读出来对应的COOKIE内容！
response.cookie:是将内容写入到客户端的COOKIE里面 这个是在浏览器内的！

安全：
xss攻击： xss就是被人恶意在程序里面执行一段js代码来窃取cookie
csrf攻击及防御手段，跨站伪造请求， 


ajax 请求
readyStatus 的5个阶段：
0: 未初始化， 还未调用open方法
1：启动状态， 调用open方法
2：发送， 调用send 方法
3：接收，已经接收到响应数据
4：完成, 已经接收到全部响应数据，而且已经可以在客户端使用了。【一般只需检查这个阶段】

jsonp 原理：利用了script 标签的src 不受同源策略的限制。

fetch
和ajax 的相同点： 都是用来做网络请求用的
fetch api 是基于promise 的设计，和 ajax  的区别
写法上：ajax是基于js的异步callback方案
                fatch是基于promise异步流程控制体验好的方案， 避免回调地狱

原生支持率并不高，幸运的是，引入下面这些 polyfill 后可以完美支持 IE8+ ：
1. 由于 IE8 是 ES3，需要引入 ES5 的 polyfill: es5-shim, es5-sham 
2. 引入 Promise 的 polyfill: es6-promise 
3. 引入 fetch 探测库：fetch-detector 
4. 引入 fetch 的 polyfill: fetch-ie8 
5. 可选：如果你还使用了 jsonp，引入 fetch-jsonp 
6. 可选：开启 Babel 的 runtime 模式，现在就使用 async/await 
* Fetch 请求默认是不带 cookie 的，需要设置 fetch(url, {credentials: 'include'}) 
* 服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。 





websocket是一种协议（h5提出的新协议）可以实现客户端和服务端的通信，实现服务端实时推送的功能

当客户端要建立Websocket连接时，其向服务器发送：
GET /chat HTTP/1.1
Host: xxx.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://xxx.com
　　其中，Upgrade: websocket和Connection: Upgrade告诉服务器，我要建立的是websocket连接；Sec-WebSocket-Key部分服务器加密后还要返回浏览器，确保建立的是websocket连接；Sec-WebSocket-Version: 13是websocket的版本号。
当服务器接收到上述包后，会返回一下内容：
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
　　它告诉客户端，我已经切换到websocket协议了，Sec-WebSocket-Accept就是Sec-WebSocket-Key加密后的内容，这样，一个websocket连接就建立了。


一些需要掌握的js api
js bind: bind函数会创建出一个新的函数，所以用bind 性能会有所下降，箭头符号恰恰可以解决这一问题

Promise: 解决回调地狱
实际例子：假设下边一个场景，我们一个服务，从一个外边service获取数据，然后写到一个db里，或者一个存储里，最后在把存储的状态龙出来，那么如果没有promise是怎么写的呢？可能会是这样：
//普通回调：
var getData = function(func) {
	setTimeout(() => {
		func(10);
	}, 1000)
}

var storeToDb = function(value, func) {
	setTimeout(() => {
		func(value + 10);
	}, 1000)
}

var logStore = function(value, func) {
	setTimeout(() => {
		func(value + 20);
	}, 1000)
}

getData(function (value1) {
  storeToDb(value1, function(value2) {
    logStore(value2, function(value3) {
     	console.log(value3);
    });
  });
});


Promise有三种状态：pending、resolved（fulfilled）、rejected
var getData = function() {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(10);
		}, 1000);	
	}) ;
};
var storeToDb =function(data) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(data + 10);
		}, 1000);	
	}) ;
}
var logStore = function(value) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(value + 10);
		}, 1000);	
	}) ;
};

getData()
	.then(data => storeToDb(data))
	.then(value => logStore(value))
	.then(value => console.log(value))
	.catch(err => console.log(“error”))
以上利用promise就避开了回调地狱的写法

Promise.all([fn1,fn2…])：
所有的promise都完成后才会调用then 方法

Promise.race([fn1,fn2…])：
Promise.race都是以一个Promise对象组成的数组作为参数，不同的是，只要当数组中的其中一个Promsie状态变成resolved或者rejected时，就可以调用.then方法了。

闭包：
首先要明白js作用域，无非是掌握全局变量和局部变量
全局变量
var a = 1;
function b()  {
	console.log(a);
}

局部变量
function  b() {
	var a = 1;
} 
console.log(a); // error

如何让局部变量能暴露出来
function b() {
	var a = 1;
	return function() {
		return a;
	}
}

var c = b();
c(); // => 1

里面的 匿名函数就是一个闭包，各种专业文献上的"闭包"（closure）定义非常抽象，很难看懂。我的理解是，闭包就是能够读取其他函数内部变量的函数。由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

自然而然，就知道了闭包的用途
（1）访问局部变量
（2）让这些局部变量保留在内存中，不释放内存

由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

var a = function() {
  var i = 0;
  for (var len=10;  i < len;  i++) {
    setTimeout(function() {
      console.log(i);
    }, 1000）
  }
};
a(); // 输出的都是10

var a = function() {
  var i = 0;
  for (var len=10;  i < len;  i++) {
    setTimeout(function(i) {
      console.log(i);
    }.bind(this, i), 1000）
  }
};
a(); // 1-10

var a = function() {
    for (let i=0, len=10;  i < len;  i++) {
        setTimeout(function() {
            console.log(i)
        },1000);
    }
};
a(); // 1-10


原型链（继承相关逻辑）



css3一些需要了解的（页面需求，node方向可跳过）


webpack gulp  grunt 三者的比较

gulp grunt 是对单独一些指定文件的工程化，而webpack是从入口文件开始，找到项目中的所有依赖，并用loaders处理，最后根据配置文件生成一个或多个js文件，从webpack的标志性logo就可以看出，不管是js还是css，结果webpack处理最终都会生成javascript文件。


webpack 功能：
生成source map 使调试更简单，

devtool : 

source-map  在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度
cheap-module-source-map   在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便；
eval-source-map   使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项；
cheap-module-eval-source-map  这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点；

webpack 提供构建本地服务器： webpack-dev-server

webpack 鼎鼎大名的Loaders
 Loaders是webpack提供的最激动人心的功能之一了。通过使用不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件，对React的开发而言，合适的Loaders可以把React的中用到的JSX文件转换为JS文件。
Loaders需要单独安装并且需要在webpack.config.js中的modules关键字下进行配置，Loaders的配置包括以下几方面：
test: 一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）
loader: loader的名称（必须
include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
query：为loaders提供额外的设置选项（可选）

常用loaders: 
处理样式    less-loader, sass-loader 
两个都必须用上。否则超过大小限制的图片无法生成到目标文件夹中 url-loader file-loader
js处理，转码: babel-loader babel-preset-es2015 babel-preset-react
1.模板:
　　　　(1)html-loader:将HTML文件导出编译为字符串，可供js识别的其中一个模块
　　　　(2)pug-loader : 加载pug模板
　　　　(3)jade-loader : 加载jade模板(是pug的前身，由于商标问题改名为pug)
　　　　(4)ejs-loader : 加载ejs模板
　　　　(5)handlebars-loader : 将Handlebars模板转移为HTML
2.样式:
　　　　(1)css-loader : 解析css文件中代码
　　　　(2)style-loader : 将css模块作为样式导出到DOM中
　　　　(3)less-loader : 加载和转义less文件
　　　　(4)sass-loader : 加载和转义sass/scss文件
　　　　(5)postcss-loader : 使用postcss加载和转义css/sss文件
3.脚本转换编译:
　　　　(1)script-loader : 在全局上下文中执行一次javascript文件，不需要解析
　　　　(2)babel-loader : 加载ES6+ 代码后使用Babel转义为ES5后浏览器才能解析
　　　　(3)typescript-loader : 加载Typescript脚本文件
　　　　(4)coffee-loader : 加载Coffeescript脚本文件
4.JSON加载:
　　　　(1)json-loader : 加载json文件（默认包含）
　　　　(2)json5-loader : 加载和转义JSON5文件
5.Files文件
　　　　(1)raw-loader : 加载文件原始内容(utf-8格式)
　　　　(2)url-loader : 多数用于加载图片资源,超过文件大小显示则返回data URL
　　　　(3)file-loader : 将文件发送到输出的文件夹并返回URL(相对路径)
　　　　(4)jshint-loader : 检查代码格式错误
6.加载框架:
　　　　(1)vue-loader : 加载和转义vue组件
　　　　(2)angualr2-template--loader : 加载和转义angular组件
　　　　(3)react-hot-loader : 动态刷新和转义react组件中修改的部分,基于webpack-dev-server插件需先安装,然后在webpack.config.js中引用react-hot-loader

一切皆模块
Webpack有一个不可不说的优点，它把所有的文件都都当做模块处理，JavaScript代码，CSS和fonts以及图片等等通过合适的loader都可以被处理。
CSS
webpack提供两个工具处理样式表，css-loader 和 style-loader，二者处理的任务不同，css-loader使你能够使用类似@import 和 url(...)的方法实现 require()的功能,style-loader将所有的计算后的样式加入页面中，二者组合在一起使你能够把样式表嵌入webpack打包后的JS文件中。
通常情况下，css会和js打包到同一个文件中，并不会打包为一个单独的css文件，不过通过合适的配置webpack也可以把css打包为单独的文件的。

CSS modules: js 模块化概念已经非常成熟，但是css 发展却相对比较缓慢，没有模块化的概念，所以css-loader将css模块化给加进去，通过配置loader,可以做到css模块化，避免全局css 选择器污染

babel 其实是一个编译javascript 的平台，它可以编译代码帮你达到以下目的：
* 让你能使用最新的JavaScript代码（ES6，ES7...），而不用管新标准是否被当前使用的浏览器完全支持；
* 让你能使用基于JavaScript进行了拓展的语言，比如React的JSX；
Babel的安装与配置
Babel其实是几个模块化的包，其核心功能位于称为babel-core的npm包中，webpack可以把其不同的包整合在一起使用，对于每一个你需要的功能或拓展，你都需要安装单独的包（用得最多的是解析Es6的babel-env-preset包和解析JSX的babel-preset-react包）。

插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西，可以这么来说，loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

Webpack有很多内置插件，同时也有很多第三方插件，可以让我们完成更加丰富的功能。
常用插件
HtmlWebpackPlugin
这个插件的作用是依据一个简单的index.html模板，生成一个自动引用你打包后的JS文件的新index.html。这在每次生成的js文件名称不同时非常有用（比如添加了hash值）。
Hot Module Replacement
Hot Module Replacement（HMR）也是webpack里很有用的一个插件，它允许你在修改组件代码后，自动刷新实时预览修改后的效果。

webpack 为开发环境下做了好多优化插件
* OccurenceOrderPlugin :为组件分配ID，通过这个插件webpack可以分析和优先考虑使用最多的模块，并为它们分配最小的ID
* UglifyJsPlugin：压缩JS代码；
* ExtractTextPlugin：分离CSS和JS文件

添加了hash之后，会导致改变文件内容后重新打包时，文件名不同而内容越来越多，因此这里介绍另外一个很好用的插件clean-webpack-plugin。

__dirname  nodejs中的全局变量，

node router 是需要学习的 

css  左右布局，高度相同

项目中了解到的：
状态机机制
npm  mime

重绘和回流：回流性能低于重绘
cmd 和 amd 
cmd 代表seajs
amd 代表requirejs
amd前置依赖 ， cmd 是懒加载依赖


redux: 函数式编程

koa 学习
koa2 中用到了async、await， 用到了es7的特性，需要node版本为7.6之后的才能支持。可以利用babel 让7.6 之前的版本支持async、await

碰到不能写的文件夹可以用：
sudo chown yangruting /usr/local/lib/pkgconfig

react: map 遇到的问题
https://stackoverflow.com/questions/29586411/react-js-is-it-possible-to-convert-a-react-component-to-html-doms
less 和 calc 冲突解决： http://blog.csdn.net/playboyanta123/article/details/50408335


Apache开启本地服务
1.启动
sudo apachectl  start
2.重新启动
sudo apachectl  restart

关闭apache：sudo apachectl stop

patch 配置文件： sudo vim /etc/apache2/httpd.conf

全局node_modules在 /usr/local/lib/node_modules下


node 的一些常用库
代码覆盖率工具： Istanbul
命令行工具： Inquirer.js  Commander
fs 扩充库： node-fs-extra



<!doctype html>
<html>
<head>
	<meta charset="utf-8" />
</head>
<body>
	<input id="test_input" type="text" />
	<button id="test_button">确定</button>
<!-- 	<script>alert(2)</script>
 -->	<script type="text/javascript">
 		var oScript = document.createElement('script');
		oScript.innerHTML = 'alert(1)';
		document.body.appendChild(oScript);
		// 以下是以外链方式加载script标签，是不会执行js的，只有上面的创建script标签的形式才会执行js
		// document.body.innerHTML = "<script>alert(1)<\/script>";
		// document.querySelector("#test_button").onclick = function() {
		// 	document.body.innerHTML = ' ' + document.querySelector("#test_input").value;
			
		// }
		
	</script>
</body>
</html>






Mac  命令集合
在Mac系统中并没有Home、End等键，所以在使用时并不是特别的顺手，但是有几个键位组合可以使Terminal的操作更加灵活方便。
1、将光标移动到行首：ctrl + a
2、将光标移动到行尾：ctrl + e
3、清除屏幕：                ctrl + l
4、搜索以前使用命令：ctrl + r
5、清除当前行：            ctrl + u
6、清除至当前行尾：    ctrl + k
7、单词为单位移动：option + 方向键


mac 的环境变量
~/ .bash_profile   // 环境变量的位置
source .bash_profile // 重启环境变量

mac   上的一些安装
adb安装
brew install android-platform-tools
git安装
在官网上下载
需要配置ssh ， 一个机器绑定一个ssh




解读react

A JAVASCRIPT LIBRARY FOR BUILDING USER INTERFACES

存在原因

> We built React to solve one problem: building large applications with data that changes over time.

mvc 架构图

以下引用自官网
React isn't an MVC framework.
React is a library for building composable user interfaces. It encourages the creation of reusable UI components which present data that changes over time.


webx+vm:
VO组织数据，VM进行展示，DOM操作变更页面

react:
Model Driven View ， 自动维护View。

react 一些基础知识

React Element  是基本单元
var React = require('react');
var root = React.createElement('div');
var ReactDOM = require('react-dom');
ReactDOM.render(root, document.getElementById('example'));

React Element by JSX JavaScript exstension 
为了结构清晰，代码可读性和可维护性， react 利用 JSX 编写 DOM 结构，可以用原生的 HTML 标签，也可以直接像普通标签一样引用 React 组件
var root = (<ul className="my-list">
             <li>Text Content</li>
           </ul>);
ReactDOM.render(root, document.getElementById('example'));

JSX 只是一个语法糖: Babel compiles JSX down to React.createElement() calls.


React Component 
组件 = 模块 = 状态机



react 三种方式定义组件
* 函数式（Functional）组件：    
       无状态组件： 适合创建纯展示的组件，这种组件只负责根据传入的props来展示，不涉及到要state状态的操作。
       function HelloComponent(props, /* context */) {
  		return <div>Hello {props.name}</div>
       }
       ReactDOM.render(<HelloComponent name="Sebastian" />, mountNode) 
       
       
       组件不会被实例化，渲染速度快
       在项目中抽象出来，得以复用

* React.createClass
       es5 react原生实现创建组件的方法
       var MyComponent = React.createClass({
            handleClick: function() {
                 console.log(this);
            },
            render: function() {
                  return <a onclick={this.handleClick}>Click Me</a>;
            } 
       }); 

       React.createClass 创建的组件，其每一个成员函数的this都有React自动绑定，任何时候使用，直接使用this.method即可，函数中的this会被正   确设置。

* React.Component
       是  react 发展过程中，用来取代  createClass 形式的方式  
       class List extends React.Component {
  		static propTypes = { data: React.PropTypes.array };
  		static defaultProps = { data: [] };
  		constructor(props) {
    			super(props);
    			this.state = {key: 'value'};
  		}
		render() { 
      			return <div onclick={this.onClick.bind(this)}></div>
  		}
	} 

        React.Component 创建的组件，其成员函数不会自动绑定this，需要开发者手动绑定，否则this不能获取当前组件实例对象。


合成事件（SyntheticEvent）
react 没有用原生的事件，而是对浏览器的原生事件做了一层封装，和 jquery 的表现一样，屏蔽浏览器兼容性的前提下保持和原生事件一样的方法。

此处需要一个源码调用图来解析一下合成事件

react 合成事件对象： SyntheticEvent.js
事件池源码：PooledClass.js

事件池中的事件是复用的，并且在事件回调函数被调用后会销毁事件对象的所有属性，以此来提高性能，所以在事件回调函数中不能异步访问事件对象。
 


Virtual DOM
顾名思义： 不是真正的DOM
```
```

```
React.createElement(
    'div',
    {
      className: css
    },
    React.createElement(
       'label',
       { className: 'item-label' },
        label
    ),
    React.createElement(
       'div',
          { className: 'item-field' },
          Inputs
    )
);

```

DOM differ
如果dom变化前后的DOM节点不一样
renderA: <div />
renderB: <span />
=> [removeNode <div />], [insertNode <span />]



浏览器渲染流程
重绘和回流 
虚拟dom 
differ
解读源码

es6
(1) 区别
Object.getOwnPropertyDescriptor()
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor
Object.getOwnPropertyDescriptor() 返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

Object.prototype.__lookupSetter__()
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/__lookupSetter__
__lookupSetter__ 方法是用来返回一个对象的某个属性上绑定了 setter （设置器）的钩子函数的引用。

以上两个方法的区别，getOwnPropertyDescriptor不会从原型链上进行查找的属性， 而__lookupSetter__会
举个栗子：
```javascript
class A  {
    set a(v) {
        this._a = v;
    }
}

class B extends A {
    set b(v) {
        this._b = v;
    }
}

Object.getOwnPropertyDescriptor(Object.getPrototypeOf(new B()), “a”); // undefined
(new B()).__lookupSetter__(“b”); // return obj
```

箭头函数不存在arguments



node 和 V8

node  查看 V8 版本号： process.versions
例如node 的版本号是6.0.0
{ http_parser: '2.7.0',
  node: '6.0.0',
  v8: '5.0.71.35',
  uv: '1.9.0',
  zlib: '1.2.8',
  ares: '1.10.1-DEV',
  icu: '56.1',
  modules: '48',
  openssl: '1.0.2g' }
而ocean机器是
{ http_parser: '2.5.0',
  node: '4.2.1',
  v8: '4.8.271.20',
  uv: '1.7.5',
  zlib: '1.2.7',
  ares: '1.10.1-DEV',
  modules: '46',
  openssl: '1.0.2e' }



  FP help
封装一些基础数据类型的常用操作，并对这些操作提供函数式编程api，链式调用语法糖，提供给开发者性能体验最优的工具集 

Array
封装一些对数组的操作，

first / last / findIndex / findLastIndex
获取数组 第一个 / 最后一个 / 从左到右满足条件的元素下标 / 从右到左满足条件的元素下标

union / uniq
数组间去重 / 数组内去重 

findIndex / findLastIndex
返回从左到右 / 从右到左遍历到的第一个符合条件的对象的数组下标

fill
用 Value 填充当前array

string
camelCase / kebabCase
转换字符串为驼峰写法 / 

trim / trimLeft / trimRIght
过滤 左右空格 / 左空格 / 右空格  

capitalize
首字母大写

// escape
// 实体字符转换

// padLeft / padRight
// 左  /  右字符串填充

数值操作
inRange
判断当前元素是否在指定范围内
random
生成一个固定范围内的随机数

类型判断：
isArray / isNumber / isString / isBoolean /  isFunction / isObject / isDate /  isRegExp /  isUndefined / isNull / 

匹配判断
isEmail / isPhone / isUrl 


Object
assign
对象扩展

keys / values
获取对象所有的键 / 值

findKey / findLastKey
从左到右 / 从右到左 获取满足条件的对象的属性

forIn / forOwn
遍历 包括原型链上可枚举的属性 / 不包括原型链上只属于自己的可枚举的属性

get
获取对象对应属性的值

has
判断是否属于对象的属性

pick
根据条件摘取对象生成新对象


function
before / after
当前函数执行 n 次前 / 后调用

bind
为函数绑定参数

curried
科里化，便于函数式编程

集合
find / findLast / findWhere
根据运算结果获取第一个 / 最后一个 符合条件的迭代元素 / 部分对象匹配

filter
过滤

forEach
遍历迭代元素

groupBy
通过运算结果对现有元素进行分组处理

every
是否每个迭代元素的运算结果都是true

map
循环迭代元素按最终运算结果生成数组

includes
判断目标元素是否在集合中

partition
根据计算结果划分集合元素

reject
获取所有不满足的对象

sample
从集合中获取随机元素

size
获取集合大小

sort 
对集合元素进行排序运算，得到排序后结果

other common use 
clone 
浅度或深度拷贝

uniqId
生成全局唯一id

range
创建一个包含指定范围的元素的数组

times
调用迭代函数指定次数

链式调用
过滤出所有不是5的倍数的值，并对这些值做乘2的处理
chain([1,6,9,8,10,45])
.filter((n) => {
	return n % 5;
})
.map((n) => {
	return n * 2;
})
.value();

问题1：如果我们的工具集是提供链式调用的，是不是 common library 所有模块都需要支持链式调用，是否需要在工具集实现一个贯穿 common library 的api, 供内部生成链式调用功能。
问题2：如果所有基础数据类型都封装成特定类型，是否基础类型的原生api都需要封装一遍



FP help base

函数式编程工具是对基础类型的常用方法做性能最优的封装，并为这些方法集合提供链式调用方案。
首先我们需要为函数式编程工具集合定义一个命名空间，就像jQuery的 $， lodash的_。命名空间将和全局保持统一

Array的常用操作
去重
differ
console.log(differ([1, 2 , 3], [6, 7, 1]));
// 2,3
drop
舍弃


findIndex / findLastIndex
返回从左到右 / 从右到左遍历到的第一个符合条件的对象的数组下标

fill
替换某一段数组为

数值操作

random
生成一个固定范围内的随机数

类型判断：
isArray / isNumber / isString / isBoolean /  isFunction / isObject / isDate /  isRegExp /  isUndefined / isNull / 

匹配判断
isEmail / isPhone / isUrl 


Object
assign
对象扩展

function
before / after
当前函数执行 n 次前 / 后调用


other common use 

find
查找
// 获取所有TextFiled TextArea 的对象或其子类的对象
let children = view.children;
find(children, (view) => {
	return view instanceof TextFiled || view instanceof TextArea
});
map
循环
// for循环的简写
map(children, (view, index) => {
	return view .id =  “view” + index;
});
filter
过滤

forEach
遍历

clone 
浅度或深度拷贝



链式调用
chain
chain(child)
.find(() => {
    return view instanceof TextFiled || view instanceof TextArea
})
.first()
.value();

函数式编程
function addMore(a) {
	return ++a;
}
map(addMore)([2,3,4])；







git 相关
（1）git ignore 失效原因及解决
规则很简单，不做过多解释，但是有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交
git rm -r --cached .
git add .
git commit -m 'update .gitignore'






冲突的产生
很多命令都可能出现冲突，但从根本上来讲，都是merge 和 patch（应用补丁）时产生冲突。
而rebase就是重新设置基准，然后应用补丁的过程，所以也会冲突。
git pull会自动merge，repo sync会自动rebase，所以git pull和repo sync也会产生冲突。当然git rebase就更不用说了。
冲突的类型
逻辑冲突
git自动处理（合并/应用补丁）成功，但是逻辑上是有问题的。
比如另外一个人修改了文件名，但我还使用老的文件名，这种情况下自动处理是能成功的，但实际上是有问题的。
又比如，函数返回值含义变化，但我还使用老的含义，这种情况自动处理成功，但可能隐藏着重大BUG。这种问题，主要通过自动化测试来保障。所以最好是能够写出比较完备的自动化测试用例。
这种冲突的解决，就是做一次BUG修正。不是真正解决git报告的冲突。
内容冲突
两个用户修改了同一个文件的同一块区域，git会报告内容冲突。我们常见的都是这种，后面的解决办法也主要针对这种冲突。
树冲突
文件名修改造成的冲突，称为树冲突。
比如，a用户把文件改名为a.c，b用户把同一个文件改名为b.c，那么b将这两个commit合并时，会产生冲突。
$ git status
    added by us:    b.c
    both deleted:   origin-name.c
    added by them:  a.c
如果最终确定用b.c，那么解决办法如下：
git rm a.c
git rm origin-name.c
git add b.c
git commit
执行前面两个git rm时，会告警“file-name : needs merge”，可以不必理会。
 
树冲突也可以用git mergetool来解决，但整个解决过程是在交互式问答中完成的，用d 删除不要的文件，用c保留需要的文件。
最后执行git commit提交即可。
内容冲突的解决办法
发现冲突
一般来讲，出现冲突时都会有“CONFLICT”字样：
$ git pull
Auto-merging test.txt
CONFLICT (content): Merge conflict in test.txt
Automatic merge failed; fix conflicts and then commit the result.
 
但是，也有例外，repo sync的报错，可能并不是直接提示冲突，而是下面这样：
error: project mini/sample
注：无论是否存在冲突，只要本地修改不是基于服务器最新的，它都可能报告这个错误，解决方法都是一样。
 
这个时候，需要进入报错的项目（git库）目录，然后执行git rebase解决：
git rebase remote-branch-name
冲突解决的一般过程
merge/patch的冲突解决
先编辑冲突，然后git commit提交。
注：对于git来讲，编辑冲突跟平时的修改代码没什么差异。修改完成后，都是要把修改添加到缓存，然后commit。
rebase的冲突解决
rebase的冲突解决过程，就是解决每个应用补丁冲突的过程。
解决完一个补丁应用的冲突后，执行下面命令标记冲突已解决（也就是把修改内容加入缓存）：
git add -u
注：-u 表示把所有已track的文件的新的修改加入缓存，但不加入新的文件。
 
然后执行下面命令继续rebase：
git rebase --continue
有冲突继续解决，重复这这些步骤，直到rebase完成。
 
如果中间遇到某个补丁不需要应用，可以用下面命令忽略：
git rebase --skip
 
如果想回到rebase执行之前的状态，可以执行：
git rebase --abort
 
注：rebase之后，不需要执行commit，也不存在新的修改需要提交，都是git自动完成。
编辑冲突的方法
直接编辑冲突文件
冲突产生后，文件系统中冲突了的文件（这里是test.txt）里面的内容会显示为类似下面这样：
a123
<<<<<<< HEAD
b789
=======
b45678910
>>>>>>> 6853e5ff961e684d3a6c02d4d06183b5ff330dcc
c
其中：冲突标记<<<<<<< （7个<）与=======之间的内容是我的修改，=======与>>>>>>>之间的内容是别人的修改。
此时，还没有任何其它垃圾文件产生。
 
最简单的编辑冲突的办法，就是直接编辑冲突了的文件（test.txt），把冲突标记删掉，把冲突解决正确。
利用图形界面工具解决冲突
如果要解决的冲突很多，且比较复杂，图形界面的冲突解决工具就显得很重要了。
 
执行git mergetool用预先配置的Beyond Compare解决冲突：
$ git mergetool
Merging:
test.txt

Normal merge conflict for 'test.txt':
  {local}: modified
  {remote}: modified
 
￼
 
上面三个窗口依次是“LOCAL”、“BASE”、“REMOTE”，它们只是提供解决冲突需要的信息，是无法编辑的。
下面一个窗口是合并后的结果，可以手动修改，也可以点击相应颜色的箭头选择“LOCAL”或者“REMOTE”。
 
在Beyond Compare中修改冲突保存后，冲突文件（test.txt）中的冲突标记就没有了，成了修改后的内容，一个文件的冲突编辑就完成了。
 
注意：
启动Beyond Compare之后，会自动生成几个包含大写字母名称、数字的辅助文件：
￼
 
关闭Beyond Compare时，这几个辅助文件都会自动删除，但同时会生成一个test.txt.orig的文件，内容是解决冲突前的冲突现场。
默认该.orig文件可能不会自动删除，需要手动删掉。


javascript 严格模式


1. 严格模式： JavaScript 在严格模式下对js代码更加苛刻
      全局变量声明显示， 不允许操作没有声明的变量： 
       "use strict";
v = 1; // 报错，v未声明
for(i = 0; i < 2; i++) { // 报错，i未声明 }
       静态绑定
       * 小插曲 with:   with语句的作用是将代码的作用域设置到一个特定的作用域中
        使用with关键字的目的是为了简化多次编写访问同一对象的工作，比如下面的例子：
	 var qs = location.search.substring(1);	
         var hostName = location.hostname;	
         var url = location.href;
        这几行代码都是访问location对象中的属性，如果使用with关键字的话，可以简化代码
          with (location){
	        var qs = search.substring(1);
	         var hostName = hostname;
         	  var url = href;
	}
        严格模式下需要在编译过程中就确定作用域，所以不允许with的存在
        创建eval作用域，eval的作用域在正常模式下取决于是在全局作用域还是函数作用域，在严格模式下都不属   于，而是在自己的作用域内
        安全措施
        禁止this作用域指向全局对象
        function f() {
                this.a = 1; 
        }
        f(); // 错误
        禁止在函数内部遍历调用债


http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html


严格模式是向ECMA标准看齐的方式，其中很多限制都是和es6甚至更高标准看齐的










