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



ES6 类似如下写法，每一次改变都是直接改变obj中的内容，所以都会改变
ES6中 import引入会提升到最前面
```js
var isRun = false;
function c() {
  if (isRun) {
    return isRun;
  }
  class A {

  }
  const obj = {};
  obj.a = new A();
  isRun = obj;
  return obj;
}
const aaa = c().a;
const aab = c().a;
aaa === aab // true, 引用同一个实例

```

Commonjs
```js
function c() {
  class A {

  }

  return {
    a: new A()
  }
}
const aaa = c().a;
const aab = c().a;
aaa === aab // false， 引用不同的实例，
```
两者转换就是类似的写法

参考es6 module模块...
