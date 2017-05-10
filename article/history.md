### react-router: `4.1.1`
// warning为false的时候会报警
// invariant为false会报警

#### react: Context

运用`Context`属性直接在react组件中传递props属性,在大多数应用中，我们基本用不到这个属性，且这个属性并不是很稳定，官方推荐类似redux等库来管理state而不是用`Context`（其实redux也是用Context使props贯穿整个应用- -）。

> Context 就近取值，从父级开始往上取，类似从原型链中取值

基本使用方法
```
// childComponent
class Child extends React.Component {
  render() {
    return (
      <div>{this.context.color}</div>
    );
  }
}

Child.contextTypes = {
  color: PropTypes.string,
};

// parentComponnet
class Parent extends React.Component {
  getChildContext() {
    return {
      color: 'blue'
    };
  }

  render() {
    return (
      <div>
        <Child />
      </div>
    );
  }
}

Parent.childContextTypes = {
  color: PropTypes.string
};
```

#### history
##### createHashHistory

返回一个history对象
```
const history = {
  length: globalHistory.length,
  action: 'POP',
  location: initialLocation,
  createHref,
  push,
  replace,
  go,
  goBack,
  goForward,
  block,
  listen
}
```

createHref @params {object} location

```
const createHref = (location) =>
  '#' + encodePath(basename + createPath(location))
```

push @params {string} path

> 1、 获取当前location

> 2、 如果hash改变，则改变url的hash值、修改allPaths[]的值，改变history中的action及location

replace @params {string} path

> 过程与`push`类似，只是使用到`location.replace()`替换当前的hash，消除当前的历史

go goBack goForward

> 与`history.go()`类似

listen

> 增加监听函数，且这些监听函数会在`setState`中一一触发

> 再次执行其结果值则可取消监听

block

> 可以设置`prompt`提示函数， 与`listen`过程类似

> 只要绑定了`block`或者`listen`，则会监听`hashchange`事件,否则取消绑定

##### createBrowserHistory

主要利用h5的API: pushState, replaceState, popstate事件
