### Rxjs记录

与Observe Pattern不同， 观察者模式是先订阅，然后传参数从清单中去执行

```js
class Producer {
	constructor() {
		this.listeners = [];
	}
	addListener(listener) {
		if(typeof listener === 'function') {
			this.listeners.push(listener)
		} else {
			throw new Error('listener 必須是 function')
		}
	}
	removeListener(listener) {
		this.listeners.splice(this.listeners.indexOf(listener), 1)
	}
	notify(message) {
		this.listeners.forEach(listener => {
			listener(message);
		})
	}
}
```

Rxjs类似与先传参数，再定义函数去执行

```js
var source = Rx.Observable
    .create(
        function(observe) {
          observe.next(1);
          observe.next(2);
        }
    );
  
source.subscribe({
    next: function(value) {
        console.log(value)
    },
    complete: function() {
        console.log('complete!');
    },
    error: function(error) {
        console.log(error)
    }
})
```