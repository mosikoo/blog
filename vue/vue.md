#### lifecycle

![](https://cn.vuejs.org/images/lifecycle.png)

#### computed vs methods

```
var a = new Vue({
  data: {
    message: 'test'
  },
  computed: {
    computeMessage: function() {
      return this.message + 'aaa'; // 有缓存
    }
  },
  methods: {
    computeMessage: function() {
      return this.message + 'aaa'; // render 触发
    }
  }
})
```

#### watche

```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

#### 绑定class

```js
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>

data: {
  isActive: true,
  hasError: false
}
```

#### 组件

全局注册

```
// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})

// 创建根实例
new Vue({
  el: '#example'
})
```

局部注册

```js
var Child = {
  template: '<div>A custom component!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})
```

模板中data必须是函数



传递的值不知道是变量还是字符串还是数字

```js
Vue.component('example', {
	propA: Number,
  	propB: {
      type: String,
      required: true，
      default: function() {
        return { message: 'hello'};
      }
  	}
})

```



#### 指令

```js
// 注册一个全局自定义指令 v-focus
Vue.directive('focus', {
  // 当绑定元素插入到 DOM 中。
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
// 局部
directives: {
  focus: {
    // 指令的定义---
  }
}

// 使用
<input v-focus>

```









### Vuex

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  // 监听 mutation 必须是同步函数
  mutations: {
    increment (state, payload) {
      state.count++
    }
  },
  // 用于快速获取处理后的变量
  getters: {
    countGetter: state => {
      return state.count++
    }
  },
  // 异步处理
  actions: {
    increment (context, payload) {
      context.commit('increment')
    }
  }
})
// 触发mutations
store.commit('increment')
// 触发actions
store.dispatch('increment')

// 使用store中的值
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }    | 等
  }      | 价
}        | 于
         v
// mapState使用, 在组件中使用
export default {
  computed: mapState([
    // 映射 this.count 为 store.state.count
    'count'
  ])
}
// 或者
export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
// state添加新属性
Vue.set(obj, 'newProp', 123)
//或
state.obj = {...state.obj, newProp: 123}

// 热重载
 store.hotUpdate({})
 
// modules
 

```

