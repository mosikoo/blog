### generator

##### 简单的demo
```js
function *demo(x) {
  const p = yield x + 1;
  yield p;
}
const result = demo(1);
result.next() // {value: 2, done: false};
result.next(33) // 这里设置p的值为33 {value: 33, done: false};
result.next() // {value: undefined, done: true}
```
* p的值为next(第二次next)中传的参数，否则为`undefined`,这里传了`33`，所以`p: 33`
* 执行next后value的值为yield后面表达式的值

##### generator与promise结合
demo
```js
function sleep(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, 2000, 'done');
  });
} 
function *demo() {
  yield sleep(2000);
}

const r = demo();
const result = r.next();
result.value.then(content => {
  console.log(content); // 'done'
});
```
* yield 传递一个promise即可

##### coroutine 协同函数
> nodeJs中比较出名是TJ的`co`
>
> yeild跟promise对象或Thunk函数

简单实现auto run
```js
 function run(fn) {
  const r = fn();
  next();
  function next(arg) {
    const result = r.next(arg);
    if (result.done) return result.value;
    result.value.then(content => {
      next(content);
    });
  }
} 
```

co简易实现，主要是思路,yield以promise为主（4.x）
```js
function co1(fn) {
  return new Promise((resolve, reject) => {
    const gen = fn();

    fullfiled();

    function fullfiled(res) {
      const ret = gen.next(res);
      next(ret);
    }

    function next(ret) {
      if (ret.done) {
        resolve(ret.value);
      } else {
        ret.value.then(fullfiled)
      }
    }
  });
}

function* testCo() {
  // const a = yield ([new Promise((res) => {
  //   setTimeout(function() {console.log(1);res('first');}, 1000);
  // }),
  // new Promise(res => { setTimeout(function() {console.log(2);res('second');}, 1000)})
  // ]);
  const a = yield (new Promise(res => { setTimeout(function() {console.log(2);res('first result');}, 1000)}));
  console.log(a, 'first');
  const b = yield new Promise(res => { setTimeout(function() {console.log(2);res('second result');}, 1000)});
  console.log(b,'second');
}

co1(testCo).then(c => {
  console.log(c)
});
```
Thunk函数