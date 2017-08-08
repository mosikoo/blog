### Tapable

> 是webpack中compiler的底层封装，是事件发布订阅执行的插件架构

简单模拟
```js
function MyClass() {
	Tapable.call(this);
}

MyClass.prototype = Object.create(Tapable.prototype);

const compiler = new MyClass();

compiler.plugin('emit', function() {

}); // 类似于观察者事件中的on
```

* plugin: 类似于定语-subscribe

* applyPlugins: 类似于发布-publish

* applyPluginsWaterfall: 瀑布串行的方式，上次执行结果作为下个函数的实参（只是替换init参数，后面参数不变），返回最终结果

* applyPluginsBailResult: 返回值不是`undefined`便返回中断函数

* applyPluginsAsync

```js
Tapable.prototype.applyPluginsAsyncSeries = Tapable.prototype.applyPluginsAsync = function applyPluginsAsyncSeries(name) {
	var args = Array.prototype.slice.call(arguments, 1);
	var callback = args.pop();
	var plugins = this._plugins[name];
	if(!plugins || plugins.length === 0) return callback();
	var i = 0;
	var _this = this;
	args.push(copyProperties(callback, function next(err) {
		if(err) return callback(err);
		i++;
		if(i >= plugins.length) {
			return callback();
		}
		plugins[i].apply(_this, args);
	}));
	//  最后一个参数为next()函数，需要在plugin中定义next才会执行下一个plugin，这就是异步
	plugins[0].apply(this, args);
};
```

* applyPluginsAsyncSeriesBailResult: 与`applyPluginsAsync`差不多，`next`函数有参数便中断执行，返回结果

* applyPluginsAsyncWaterfall(name, init, next): 异步串行，value为第二个参数会代替init的值

* applyPluginsParallel: 异步并行，remaining为0时说明所有异步执行完毕

* applyPluginsParallelBailResult: 参数会依次增加，调用最后一个参数执行回调，回调的参数不为空的时候，会执行callback,且后续不会执行






