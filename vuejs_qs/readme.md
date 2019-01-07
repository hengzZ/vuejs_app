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
注意事项：
* el - DOM元素绑定
* data 对象
* $data - vue成员对象引用