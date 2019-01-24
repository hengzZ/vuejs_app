## v-bind 绑定 Class 与 Style

###### 操作元素的 class 列表、内联样式（css），它们都是属性，是数据绑定的一个常见需求。

###### 不过，字符串拼接麻烦且易错。因此，在将 v-bind 用于 class 和 style 时，Vue.js 做了专门的增强。


#### 1. 绑定 HTML Class

##### 1.1 传给 v-bind:class 一个对象，以动态地切换 class
```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
和如下 data：

data: {
  isActive: true,
  hasError: false
}

结果渲染为：
<div class="static active"></div>
```
绑定的数据对象不必内联定义在模板里:
```html
# 这里绑定一个返回对象的计算属性

<div v-bind:class="classObject"></div>

// Vue 实例
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

##### 1.2 把一个数组传给 v-bind:class，以应用一个 class 列表
```html
<div v-bind:class="[activeClass, errorClass]"></div>

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
根据条件切换列表中的 class:
```html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

##### 1.3 用在组件上

###### 当在一个自定义组件上使用 class 属性时，这些类将被添加到该组件的根元素上面。
###### 注意： 这个元素上已经存在的类不会被覆盖!!!
```html
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
然后在使用它的时候添加一些 class：

<my-component class="baz boo"></my-component>

HTML 将被渲染为:
<p class="foo bar baz boo">Hi</p>
```
带数据绑定 class 也同样适用：
```html
<my-component v-bind:class="{ active: isActive }"></my-component>

当 isActive 为 truthy[1] 时，HTML 将被渲染成为：
<p class="foo bar active">Hi</p>
```


#### 2. 绑定内联样式 (css style)

##### 2.1 传给 v-bind:style 一个对象
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

data: {
  activeColor: 'red',
  fontSize: 30
}

// 直接绑定到一个样式对象
<div v-bind:style="styleObject"></div>

data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
同样的，对象语法常常结合返回对象的计算属性使用。

##### 2.2 传给 v-bind:style 一个数组
```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
