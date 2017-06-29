#### commonJs与ES6 Module
> CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

> CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

CommonJs是值的拷贝
```js
var counter = 3;
var obj = {
  counter: 3
};
function incCounter() {
  obj.counter++;
  counter++;
}
module.exports = {
  counter,
  obj,
  incCounter,
};
```

任何文件引入`let counter = require('./lib');`,无论怎么改变，在另一个文件中都不会变，有缓存机制


参考es6 module模块...
