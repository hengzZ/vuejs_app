# Vue Instance

###### 每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的
```html
var vm = new Vue({
  // 选项
})

# 经常使用 vm (ViewModel 的缩写) 这个变量名表示 Vue 实例
```

当创建一个 Vue 实例时，你可以传入一个**选项对象**。作为参考，你也可以在 API 文档(https://cn.vuejs.org/v2/api/#选项-数据) 中浏览完整的选项列表。


#### 1. 数据与方法

###### 值得注意的是只有当实例被创建时 data 中存在的属性才是响应式的。
```html
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3

## 当数据 a 改变时，视图会进行重渲染。现在，你添加一个新的属性
vm.b = 'hi'
那么对 b 的改动将不会触发任何视图的更新。

[结论] 如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，
       那么你需要提前声明并设置一些初始值。
```

###### Object.freeze() 阻止修改现有的属性，也意味着响应系统无法再追踪变化。
```html
// Vue 实例
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})

// HTML 网页
<div id="app">
  <p>{{ foo }}</p>
  <!-- 这里的 `foo` 不会更新！ -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

###### 除了数据属性，vue 实例还包含一些有用的实例属性与方法，它们都有前缀 $，以便与用户定义的属性区分开来。


#### 2. Vue 实例生命周期

###### 生命周期函数就是 vue 实例在某一个时间点会自动执行的函数，这给了用户在不同阶段（时刻）添加自己的代码的机会。

###### 生命周期钩子的 this 上下文指向调用它的 Vue 实例。
```html
不要在选项属性或回调上使用**箭头函数**，比如
 created: () => console.log(this.a) 或
 vm.$watch('a', newValue => this.myMethod())。
因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例，经常导致
 Uncaught TypeError: Cannot read property of undefined 或
 Uncaught TypeError: this.myMethod is not a function 之类的错误。
```

##### 生命周期图示
请看：https://cn.vuejs.org/v2/guide/instance.html#生命周期图示
