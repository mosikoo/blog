### 理解Js对象及原型链

#####  `_proto_`
  每个js对象都有原型对象`_proto_`，用来从原型链上继承属性与方法。对象通过`_proto_`寻找上游对象，因为形成原型链，直到Object.prototype(或者null)达到顶层

##### prototype
只有函数才有prototype属性， 当通过`new`关键字来调用创造该构造函数的实例时，该实例会自动继承prototype的属性与方法

##### instanceof方法
A instanceof B: 判断A是否是B的实例或者判断A的原型链的上层是否为B
等价于： A.\_proto\_ === B.prototype
