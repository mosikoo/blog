对于webpack一直都没有深入研究，业务上都在使用[nowa](https://github.com/nowa-webpack/nowa)构建工具来构建整个项目，webpack配置项都隐藏在[nowa-server](https://github.com/nowa-webpack/nowa-server)工具之中,所以在构建业务项目这块并没有很在意。

随着业务项目的扩大,build与server的时间越来越长(build长至80s,server的热插播也要40s)。换句话说,随意改点样式rebuild都是需要等待40s，这种等待在开发中是完全无法忍受的。

每个业务页也就几百行代码，打包体积不至于大至几M，思考一番后进行以下几点优化：

#### 使用公用CDN或者单独引包
之前引入Uxcore的方式都是这样的

```
import { Button } from 'Uxcore'
```
导致每个组件都直接引入了Uxcore整个包，是使得bundle体积过大的直接原因

所以解决方法是使用 externals 声明一个外部依赖

```
// 在webpack.config.js中修改
  externals: {
    Uxcore: 'Uxcore'
  }
```

或者进行单独引包,如：

```
import Button from 'uxcore-button'
```

优化之后,build时间从**44s**减少至**30s**，server时间从**40s**减少至**20s**

#### 移除pages中无用页面
我们的业务是多页面形式，nowa会把'src/pages'下的每个目录都进行打包。


而我们的代码有「页面嵌入页面」的形式，导致pages下有些页面不会单独出现，自然而然不需要单独打包输出。因此移除这些无用的页面。

继以上优化之后，打包时间减少至**18s**, server时间从**20s**减少至**15s**

##### build时间优化过程

![webpack-nowa-build](https://raw.githubusercontent.com/mosikoo/blog/master/mock/webpack/build.png)

##### server时间优化过程

![webpack-nowa-server](https://github.com/mosikoo/blog/raw/master/mock/webpack/server.png)

#### nowa-server 配置是否可调试
打包时间在可接收范围之内，但是开发时每次修改一点点内容热更新都需要15s时间还是忍无可忍。
最后发现是nowa-server中`webpack.SourceMapDevToolPlugin`插件原因，因为调试需要sourcemap，导致bundle体积过大，热更新速度减慢。

所以顺带提了pr使是否调试为可选项，如果开发过程中不需要调试的时候执行

```
nowa-server --no-sourcemap
```
关闭调试的功能

实测原项目server只要**5-6s**，热更新基本只要**1s**

![webpack-rebuild](https://raw.githubusercontent.com/mosikoo/blog/master/mock/webpack/rebuild.png)



#### 总结

此次优化的点不多，但是能让rebuild时间从40s减少到1s还是挺满意的，至少再也不用等待那么长的时间~

所以建议采购组的同学都升级下`nowa-server`
```
nowa install nowa-server
```

在不需要调试的情况下,加`--no-sourcemap`起服务，带来飞一般的体验

```
nowa-server --no-sourcemap
```
