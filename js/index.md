# js专题

> 函数式编程思想，为前端工程提供了性能优化的可能

## 尾调化优化

> 尾调化是指某个函数在最后调用另外一个函数

具体形式如下所示

``` javascript
function f() {
	return g(x);
}

```

f在调用g的时候，会形成一个调用栈，一般情况下调用完了g，释放了g的内存后才会释放f函数的内存。但是如果是上述情况，则会率先释放f函数，然后才开始调用g函数，这样将大大节省内存空间,也不会发生栈溢出。<br/>
因而在性能优化上引起尾递归的概念
####尾递归
众所周知，递归是非常耗内存，因为调用记录一直保存内存中。但是尾递归会对性能有一个极大的提升<br/>
阶乘函数

``` javascript
var factorial = (n) => {
  if(n === 1) return 1;
  return n * factorial(n-1);
}
```
复杂度：O(n);<br/><br/>
尾递归后：

```javascript
var factorial = (n, total=1) => {
  if(n == 1) {
    return total;
  }
  total = n * total;
  return factorial(n-1, total);
}
```
复杂度为O(1)


##函数柯里化
>意义在于把函数完全变成"接受一个参数，返回一个值的固定形式"

常见的柯里化的例子

```javascript
var foo = function(a, b) {
	return a+b;
}

var bar = foo.bind(null, 1);
bar(2);
```
或者

```javascript
var foo = function(a) {
	return function(b) {
		return a + b;
	}
}
var bar = foo(1);
bar(2);
```
顺带提下ES5中bind实现的简单原理

```javascript
var bind = function(context ,fn) {
	return fn.apply(context, arguments);
}
```

## in与hasOwnProperty

> hasOwnProperty检测对象的自有属性，不会到对象原型中查找，IN会到对象原型链依次查找

```javascript
var Person = function(name) {
  this.name = name;
}
Person.prototype = {
  sayName: function() {
    console.log(this.name);
  }
};
// 原型
var person = new Person('tom');

console.log(person.hasOwnProperty('sayName')); // false
console.log('sayName' in person); // true

// 继承
var Student = function(name, score) {
  Person.call(this, name);
  this.score = score;
};
Student.prototype = new Person();
var student = new Student('name', 11);

console.log(student.hasOwnProperty('sayName')); // false
console.log('sayName' in student); // true
```

### closure

```javascript
function closure(num) {
  const arr = [1, 2, 3];
  function dosomething(i) {
    num += i;
    arr.push(num);
    console.log(num, arr);
  }
  return dosomething;
}

var fooo = closure(2);  // 生成新的闭包环境
fooo(4); // 6, [1, 2, 3, 6]
fooo(5); // 11, [1, 2, 3, 6, 11]
```

### 深度拷贝原始数据
```
var dataSource = JSON.parse(JSON.stringify(source))
```


### 设置属性Object.defineProperty
用`Object.defineProperty`设置obj的属性可以不用顾虑`setter`
```javascript
var obj = {
    a: 10,
    get name () {
        return `name: ${this._name_}`;
    },
    set name (x) {
        this._name_ = `name: ${x}`;
    }
};
// 两种方式都可以
// Object.defineProperty(obj, 'name', {
//     get() {
//         return this._name_;
//     },
//     set: function(x) {
//         this._name_ =  `name: ${x}`;
//     }
// });
obj.name = 'xxx';

console.log(obj.name);

Object.defineProperty(obj, 'name', {
    value: 'tom'
});
console.log(obj.name);
```

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

##### demo

```javascript
class A {}
class B extends A {}
var a = new A();
var b = new B();
b.__proto__ === B.prototype // true
a.__proto__ === A.prototype // true
B.prototype.__proto__ === A.prototype // true 继承的原型链
```

##### new 一个对象的必要性

假设写一个Dialog，如果单纯用function是无法完成的，创造类后有this可以互相调用
new 一个Dialog实例（如`var dialog = new Dialog(opt)`）,
吊销dialog就需要直接调用dialog.destroy，是吊销这个dialog而不是另外一个实例，只因为this的存在.

其他的作用？？
写一个简单的dialog 放到这里中介绍
