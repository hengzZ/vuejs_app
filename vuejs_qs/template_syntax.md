# Vue Template Syntax

###### 如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，直接写渲染 (render) 函数，使用可选的 JSX 语法。

* Part 1. 插值
* Part 2. 指定
* Part 3. 缩写

#### 1. 插值

###### 1) 数据绑定最常见的形式就是使用 “Mustache” 语法 (双大括号) 的文本插值。

###### 2) 双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令。
```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>

[输出]
Using mustaches: <span style="color:red">This should be red.</span>
Using v-html directive: This should be red.

# 这个 span 的内容将会被替换成为属性值 rawHtml，
  直接作为 HTML——会忽略解析属性值中的数据绑定
# 请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值!!!
```

###### 3) Mustache 语法不能作用在 HTML 特性(attr)上，遇到这种情况应该使用 v-bind 指令。
```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

###### 4) 实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。
***每个绑定都只能包含单个表达式。***
```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```


#### 2. 指令
***指令 (Directives) 是带有 v- 前缀的特殊特性。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。***

###### 一些指令能够接收一个“参数”，在指令名称之后以冒号表示。
```html
<a v-bind:href="url">...</a>
# 在这里 href 是参数，告知 v-bind 指令将该元素的 href 特性与表达式 url 的值绑定。

<a v-on:click="doSomething">...</a>
# 在这里参数是监听的事件名。
```

###### 修饰符 (Modifiers) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
```html
<form v-on:submit.prevent="onSubmit">...</form>
# 在这里 .prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()。
```

#### 3. 缩写

###### v- 前缀作为一种视觉提示，用来识别模板中 Vue 特定的特性。当你在使用 Vue.js 为现有标签（tag）添加动态行为 (dynamic behavior) 时，v- 前缀很有帮助。 ***(Html Website)***

###### 但是，在构建由 Vue.js 管理所有模板的单页面应用程序 (SPA - single page application) 时，v- 前缀也变得没那么重要了。 ***(App)***

###### 因此，Vue.js 为 v-bind 和 v-on 这两个最常用的指令，提供了特定简写。
