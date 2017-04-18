### 理解Js对象及原型链

#####  `_proto_`
  每个js对象都有原型对象`_proto_`，用来从原型链上继承属性与方法。对象通过`_proto_`寻找上游对象，因为形成原型链，直到Object.prototype(或者null)达到顶层

##### prototype
只有函数才有prototype属性， 当通过`new`关键字来调用创造该构造函数的实例时，该实例会自动继承prototype的属性与方法

##### instanceof方法
A instanceof B: 判断A是否是B的实例或者判断A的原型链的上层是否为B
等价于： A.\_proto\_ === B.prototype

##### 各个对象间原型示例图

![proto.png](../assets/proto.png)


##### constructor是什么？？

##### new 一个对象的必要性

假设写一个Dialog，如果单纯用function是无法完成的，创造类后有this可以互相调用
new 一个Dialog实例（如`var dialog = new Dialog(opt)`）,
吊销dialog就需要直接调用dialog.destroy，是吊销这个dialog而不是另外一个实例，只因为this的存在.

其他的作用？？
写一个简单的dialog 放到这里中介绍
