延迟

```
import { Toast } from '@ali/cg-react';

let timing;

const keys = ['info', 'error', 'success'];
const noDuplToast = {};

keys.forEach(key => {
  noDuplToast[key] = (message, time = 1500) => {
    !timing && Toast[key](message, time/1000); //  表示可以操作
    if (!timing) {  // 然后加上延时
      timing = setTimeout(() => {
        timing = null;   // 时间到了后为null，然后可以继续操作
      }, time);
    }
  }
})

export default noDuplToast;

```