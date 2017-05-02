### react-router: `4.1.1`
// warning为false的时候会报警
// invariant为false会报警

#### react: Context

运用`Context`属性直接在react组件中传递props属性,在大多数应用中，我们基本用不到这个属性，且这个属性并不是很稳定，官方推荐类似redux等库来管理state而不是用`Context`（其实redux也是用Context使props贯穿整个应用- -）。

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
```
invariant(
  canUseDOM,
  'Hash history needs a DOM'
) // 需要一个有DOM的环境，指浏览器环境
```
