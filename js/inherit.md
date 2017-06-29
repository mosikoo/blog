### 继承写法

看了babel转jsx后的源码的写法，觉得这样写非常合理，且用了`__proto__`继承了静态属性

上源码

```js
var _createClass = function () {
  function defineProperties(target, props) {
    for (var i = 0; i < props.length; i++) {
      var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false;
      descriptor.configurable = true;
      if ("value" in descriptor)
        descriptor.writable = true;
      Object.defineProperty(target, descriptor.key, descriptor); 
    } 
  } 

  return function (Constructor, protoProps, staticProps) {
    if (protoProps)
      defineProperties(Constructor.prototype, protoProps); 
    if (staticProps)
      defineProperties(Constructor, staticProps);
    return Constructor;
  };
}();

function _classCallCheck(instance, Constructor) {
  // 检车格式
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

function _possibleConstructorReturn(self, call) { 
  if (!self) { 
    throw new ReferenceError("this hasn't been initialised - super() hasn't been called"); 
  } 
  return call && (typeof call === "object" || typeof call === "function") ? call : self; 
}

function _inherits(subClass, superClass) { 
  // 检验
  if (typeof superClass !== "function" && superClass !== null) { 
    throw new TypeError("Super expression must either be null or a function, not " + typeof superClass); 
  } 
  // 继承原型
  subClass.prototype = Object.create(superClass && superClass.prototype, 
    { constructor: { value: subClass, enumerable: false, writable: true, configurable: true } }
  );
  // 继承静态属性
  if (superClass)  
    Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
}

var DemoPage = function (_Component) {
  _inherits(DemoPage, _Component); // prototype及__proto__继承

  function DemoPage(props) {
    _classCallCheck(this, DemoPage);

    var _this = _possibleConstructorReturn(this, 
      (DemoPage.__proto__ || Object.getPrototypeOf(DemoPage)).call(this, props)
    ); // 借用构造函数

    _this.state = {
      name: 1
    };
    return _this;
  }

  _createClass(DemoPage, [{
    key: 'componentDidUpdate',
    value: function componentDidUpdate() {
      console.log(12);
    }
  }, {
    key: 'test',
    value: function test() {
      this.setState({
        name: 2
      });
    }
  }, {
    key: 'render',
    value: function render() {
      var demo = _react2.default.createElement(
        _componentDemo2.default,
        { value: 'test' },
        _react2.default.createElement(
          'div',
          null,
          '1'
        ),
        _react2.default.createElement(
          'div',
          null,
          _react2.default.createElement('input', null)
        )
      );
      return _react2.default.createElement(
        'div',
        { onClick: this.test.bind(this) },
        _react2.default.createElement(
          'a',
          null,
          '1'
        ),
        demo
      );
    }
  }]);

  return DemoPage;
}(_react.Component);
```