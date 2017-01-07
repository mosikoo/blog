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