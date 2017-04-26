### 全局作用域下的配置

> script中引入， 配置全局别名，能够有效地减少打包体积

```
  config.externals = {
    '@ali/cg-react': 'var CGReact',
    'react': 'var React',
    'react-dom': 'var ReactDOM'
  };
```

### 设置别名

> 设置别名，

```
    config.resolve.alias = {
      react: path.join(__dirname, 'node_modules', 'react'),
      'react-dom': path.join(__dirname, 'node_modules', 'react-dom')
    };

    if (process.env.NODE_ENV === 'develop') {
      // debug模式
      config.devtool = 'eval-source-map';
    }
```

### 热替换

```
npm i webpack express webpack-dev-middleware webpack-hot-middleware -D
```

webpack.config.js配置项中
```
entry: [
  'webpack/hot/dev-server',
  'webpack-hot-middleware/client',
  'client/index.js'
],
plugins: [
  new webpack.HotModuleReplacementPlugin()
]
```


### loader整理

### plugins整理
