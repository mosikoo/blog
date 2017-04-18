#### 问题
onMouseEnter(e) {}
e具体指什么，文档参考？



多种参数下 选择不传递

```
renderTreeData(props) {
    const validProps = props || this.props;
}
```

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
