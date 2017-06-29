#### pooledClass.js文件源码

> 构建静态池,主要作用是节省开销？ 意义是管理实例数据的创建和销毁

基本用法
```js
function Test(name) {
  this.name = name;
}
Test.prototype.destructor = function() {
  this.name = null;
}
addPoolingTo(Test); // 此时Test拥有4个key：instancePool, poolSize, getPooled, release
// 增加实例
var a = Test.getPooled(name1);
var b = Test.getPooled(name2);
Test.release(a);
Test.release(b);
```
```js
'use strict';

var invariant = require('fbjs/lib/invariant');
// 根据不同的参数构建多个getPooled函数，one-four，代码基本一致

var oneArgumentPooler = function(copyFieldsFrom) {
  var Klass = this;
  // 新建的时候会查看实例池中是否存在，存在则用原来的实例重新构建
  if (Klass.instancePool.length) {
    var instance = Klass.instancePool.pop();
    Klass.call(instance, copyFieldsFrom);
    return instance;
  } else {
    return new Klass(copyFieldsFrom);
  }
};

var twoArgumentPooler = function(a1, a2) {
  var Klass = this;
  if (Klass.instancePool.length) {
    var instance = Klass.instancePool.pop();
    Klass.call(instance, a1, a2);
    return instance;
  } else {
    return new Klass(a1, a2);
  }
};

var threeArgumentPooler = function(a1, a2, a3) {
  var Klass = this;
  if (Klass.instancePool.length) {
    var instance = Klass.instancePool.pop();
    Klass.call(instance, a1, a2, a3);
    return instance;
  } else {
    return new Klass(a1, a2, a3);
  }
};

var fourArgumentPooler = function(a1, a2, a3, a4) {
  var Klass = this;
  if (Klass.instancePool.length) {
    var instance = Klass.instancePool.pop();
    Klass.call(instance, a1, a2, a3, a4);
    return instance;
  } else {
    return new Klass(a1, a2, a3, a4);
  }
};

var standardReleaser = function(instance) {
  var Klass = this;
  invariant(
    instance instanceof Klass,
    'Trying to release an instance into a pool of a different type.',
  );
  instance.destructor();
  // 释放时候实例池未满则push,getPooled的时候再加以利用
  if (Klass.instancePool.length < Klass.poolSize) {
    Klass.instancePool.push(instance);
  }
};

var DEFAULT_POOL_SIZE = 10;
var DEFAULT_POOLER = oneArgumentPooler;

type Pooler = any;

/**
 * Augments `CopyConstructor` to be a poolable class, augmenting only the class
 * itself (statically) not adding any prototypical fields. Any CopyConstructor
 * you give this may have a `poolSize` property, and will look for a
 * prototypical `destructor` on instances.
 *
 * @param {Function} CopyConstructor Constructor that can be used to reset.
 * @param {Function} pooler Customizable pooler.
 */
var addPoolingTo = function<T>(
  CopyConstructor: Class<T>,
  pooler: Pooler,
): Class<T> & {
  getPooled(
    ...args: $ReadOnlyArray<mixed>
  ): /* arguments of the constructor */ T,
  release(instance: mixed): void,
} {
  // Casting as any so that flow ignores the actual implementation and trusts
  // it to match the type we declared
  var NewKlass = (CopyConstructor: any);
  NewKlass.instancePool = [];
  NewKlass.getPooled = pooler || DEFAULT_POOLER;
  if (!NewKlass.poolSize) {
    NewKlass.poolSize = DEFAULT_POOL_SIZE;
  }
  NewKlass.release = standardReleaser;
  return NewKlass;
};

var PooledClass = {
  addPoolingTo: addPoolingTo,
  oneArgumentPooler: (oneArgumentPooler: Pooler),
  twoArgumentPooler: (twoArgumentPooler: Pooler),
  threeArgumentPooler: (threeArgumentPooler: Pooler),
  fourArgumentPooler: (fourArgumentPooler: Pooler),
};

module.exports = PooledClass;
```
