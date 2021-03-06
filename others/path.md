## 环境变量设置

> 不同环境中设置不同的变量来区别配置文件

安装 cross-env 兼容win与mac

```javascript
npm install cross-env --save-dev
```

在package.json的stricps设置，例如

```javascript
"scripts": {
    "start": "cross-env NODE_ENV=develop bisheng start"
  }
```

然后可以在node文件中引用

```javascript
if (process.env.NODE_ENV === 'develop') {
  // debug模式
  config.devtool = 'eval-source-map';
}
```

webpack中也可以设置常量

```javascript
plugins: [
	new webpack.DefinePlugin({
	  'process.env': {
	     NODE_ENV: JSON.stringify('development'),
	     test111: JSON.stringify('test')
	  }
	})
]
```
然后可以在js文件中使用

```javascript
console.log(process.env.test111);
console.log(process.env);
```

```
// 是否是开发环境
export const DEV_ENV = (             
  !isNaN(location.hostname.replace(/\./g, '')) || location.hostname === 'localhost'
);

// xxxx表示特定的网站,是否是测试环境
export const TEST_ENV =( 
   location.hostname.indexOf('xxxx') > -1
);

// 是否是生产环境
export const PRODUCTION_ENV = (
  !DEV_ENV && !TEST_ENV
);

```
