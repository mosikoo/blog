## babel

> 平时就配脚手架的时候用到，之后用到的不多。，这次开发发布工具`aopu`时用babel转码遇到不少坑，因此简单记录下
 
#### babel-cli
用于命令行转码

一般全局安装
```bash
npm i babel-cli -g
```

用法
```bash
# 将a.js输出到指定文件
babel a.js -o b.js

# 对整个目录进行转码
bable src -d dist
```
#### .bablerc配置文件
置于根目录下，所有的babel都基于此文件执行

格式
```js
{
  presets: ["es2015", "stage-0"],
  plugins: []
}
```

#### babel-polyfill
在头部引入,用于对一些新的API进行转码
```js
require('bable-polyfill')
```

#### babel-core
6.X版本之后默认提供`babel-core`，作用是把 js 代码分析成 ast，方便各个插件分析语法进行相应的处理


#### demo
此次开发node工具,其中引入了`async/await`, 因此需要`babel`进行编译。因此引入一以下代码作为入口文件（如果不是，则无法编译），
```js
#! /usr/bin/env node

require("babel-core/register");
require("babel-polyfill");
require('./lib/aopu');
```

`require("babel-core/register");`只能用于开发中，一是因为全局安装，不编译`node_modules`中的文件，而全局安装后，代码全部在`node_modules`中导致失效;二是性能不及已打包好的方式。

因此production模式下直接执行
`babel src -d dist` 即可






