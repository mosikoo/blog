## JSONP

>  JSONP就是利用\<script\>标签的跨域能力实现跨域数据的访问。
> 
>  但是JSONP只支持 GET 请求

其实不仅script标签拥有跨域能力， 只要带src的标签都带有跨域能力

---

在js加载完了后会触发
```
script.onreadystatechange = script.onload = function() {}
```

jsonp过程

1、设置回调函数

```
window.callback= function back(resp) {
    resolve(resp); // 处理resp
}
```

2、添加script标签，设置onreadystatechange与onload函数（触发后删除此标签）

3、设置src

```
script.src = `xxxx.jsonp?callback=callbackName`;
```

**callback**是前后端约定的自定，一般都是callback

4、在设置完src后，加载了script后触发后端返回的函数

假设后端返回了

```
callbackName ({
	name: 'tom',
	age: 11
})
```

然后调用了前端设置的函数

---
详细代码看utils项目中的[fetch]()




