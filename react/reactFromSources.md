#####生命周期调用

```
react调用

/* 第一次 render */
getDefaultProps  
getInitialState  
componentWillMount  
render  
componentDidMount  
/* 第二次 render */
componentWillReceiveProps(nextPorps)     只有这里才能用setState， 以下函数用死循环
这个函数主要是为了更新某些state，（这些state初始化的时候是根据props来设置的）
shouldComponentUpdate (nextProps, nextState)
componentWillUpdate(nextProps, nextState)
render  
componentDidUpdate(prevProps, prevState )

/* mount */
getDefaultProps  
getInitialState  
componentWillMount  
render  
componentDidMount  
/* unmount */
componentWillUnmount

```
##### 延时函数

```
function buffer(fn, ms) {
  let timer;

  function clear() {
    if (timer) {
      clearTimeout(timer);
      timer = null;
    }
  }

  function bufferFn() {
    clear();
    timer = setTimeout(fn, ms);
  }

  bufferFn.clear = clear;

  return bufferFn;
}
```

##### 创建与移除组件

> 除非是顶层创建或移除组件，否则没必要用到下面这些函数

```
function removeContainer(instance) {
  if (instance._container) { // instance._container 是要移除的dom元素
      const container = instance._container;
      ReactDOM.unmountComponentAtNode(container);
      container.parentNode.removeChild(container);
      instance._container = null;
  }
}

function getContainer(instance) { // instance 为 this
    const popupContainer = document.createElement('div');
    const mountNode = document.body;
    mountNode.appendChild(popupContainer);
    return popupContainer;
}

function renderComponent() {
  // instance 为组件，instance_container为容器 两者区别比较
  ReactDOM.unstable_renderSubtreeIntoContainer(instance,
    component, instance._container,
    function callback() {
      instance._component = this;
      if (ready) {
        ready.call(this);
      }
    });
}
```

##### 获取自身

```
ReactDOM.findDOMNode(this)
```

### 源码

ReactBaseClasses文件定义`React.Component`、`React.PureComponent`,用于继承
```javascript
function ReactComponent(props, context, updater) {
  this.props = props;
  this.context = context;
  this.refs = emptyObject;
  // We initialize the default updater but the real one gets injected by the
  // renderer.
  this.updater = updater || ReactNoopUpdateQueue;
}
// prototype定义了`setState`, `forceUpdate`, `isReactComponent`
```

ReactElement.js文件
构造React元素
```javascript
ReactElement(type, key, ref, self, source, owner, props)
ReactElement.cloneElement = function(element, config, children) {} // React.cloneElement(<Demo />, {value: 10}, children);
ReactElement.createElement = function(type, config, children) {} // React.createElement(Demo, {value: 10}, children);
```

ReactChildren.js文件
  **traverseAllChildren.js下迭代器遍历`getIteratorFn`？？

traverseAllChildren.js 主要是用于递归遍历+回调
ForEachBookKeeping： 回调的函数来自于ForEachBookKeeping结合`PooledClass.js`中构建的实例，在最后会将实例保存下来。第二次forEach遍不会重复new（可能为了节省开销）
ForEachBookKeeping： 比ForEachBookKeeping复杂，对于result结果，如果是数组或者是react对象，会再一次深入遍历，其他结果类似。

代码很绕，应该再看一遍分析思路


createClass.js文件
引用了`create-react-class/factory`包
这个js会返回`createClass`函数
调用`createClass(spec)`返回自定义构建的`Constructor`类（也就是组件），
再根据这个类构建实例

主要以下三件事
* 定义构造方法Constructor，构造方法中进行props，refs等的初始化，并调用getInitialState来初始化state
* 调用getDefaultProps，并放在defaultProps类变量上。这个变量不属于某个单独的对象。可理解为static 变量
* 将React中暴露给应用，但应用中没有设置的方法，设置为null。

ReactUpdates.js文件
```js
var ReactUpdates = {
  ReactReconcileTransaction: null,
  batchedUpdates: batchedUpdates, // 执行分批策略函数
  enqueueUpdate: enqueueUpdate,
  flushBatchedUpdates: flushBatchedUpdates,
  injection: ReactUpdatesInjection, // 注射函数
};
```
