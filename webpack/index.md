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

入口文件中
```
if (module.hot) {
  module.hot.accept();
}
```

server.js中进行中间件配置

### output中
hash是全局的，只要一个file改变，hash都会改变，所以使用意义不大
一般用`chunkhash`, 表示chunk的hash值
publicPath: 该选项的值是以 runtime(运行时) 或 loader(载入时) 所创建的每个 URL 为前缀.
```js
// 当设置这样时
publicPath: "/assets/"
// 加载
background-image: url(/spinner.gif);
// 自动加上前缀
background-image: url(/assets/spinner.gif);

```
### loader整理
url-loader
file-loader
css-loader
style-loader
bable-loader
postcss-loader
file-loader 可以指定要复制和放置资源文件的位置，以及如何使用版本哈希命名以获得更好的缓存。此外，这意味着 你可以就近管理你的图片文件，可以使用相对路径而不用担心布署时URL问题。使用正确的配置，Webpack 将会在打包输出中自动重写文件路径为正确的URL。

url-loader 允许你有条件将文件转换为内联的 base-64 URL（当文件小于给定的阈值），这会减少小文件的 HTTP 请求。如果文件大于该阈值，会自动的交给 file-loader 处理。
### plugins整理
