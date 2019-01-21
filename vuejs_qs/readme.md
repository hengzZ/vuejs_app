# VueJS Quick Start
官方教程，请参考：https://cn.vuejs.org/v2/guide

## Your First Vue
```html
<!doctype html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Hello, World</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>

<div id="app">{{content}}</div>

<div>{{content}}</div>

<script>
    // var dom = document.getElementById("app")
    // dom.innerHTML = "hello, world"

    // setTimeout(function() {
    //     dom.innerHTML = "bye, world"
    // }, 2000)

    var app = new Vue({
        el: "#app",
        data: {
            content: "hello, vuejs"
        }
    })

    setTimeout(function() {
        app.$data.content = "bye, vuejs"
    }, 2000)
</script>
</body>
</html>
```
code link: [helloworld.html](./helloworld.html)

注意事项：
* el - DOM元素绑定
* data 对象
* $data - vue成员对象引用

## TodoList Project
Contents:
* list object - vue 列表数据
* v-for="item in list" - 列表遍历
* v-on:[event]="functionName" - 事件绑定
* methods - vue 函数成员
* v-model="inputValue" - 数据的双向绑定
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>TodoList</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <input type="text" v-model="inputValue">
    <button v-on:click="handleBtnClick">提交</button>
    <ul>
        <li v-for="item in list">{{item}}</li>
    </ul>
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            list: ["item 1", "item 2"],
            inputValue: ""
        },
        methods: {
            handleBtnClick: function() {
                this.list.push(this.inputValue);
                //alert(this.inputValue)
                this.inputValue = "";
            }
        }
    })
</script>
</body>
</html>
```
code link: [practices/TodoList.html](./practices/TodoList.html)

