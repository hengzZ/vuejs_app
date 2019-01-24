# VueJS Quick Start
官方教程，请参考：https://cn.vuejs.org/v2/guide

###### Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统


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


## Vue - A TodoList Project
Contents:
* list object - vue 列表数据
* v-for="item in list" - 列表遍历
* v-on:[event]="functionName" - 事件绑定
* methods - vue 函数成员
* v-model="inputValue" - 数据的双向绑定

###### v-on 指令添加一个事件监听器是实现用户交互的重要手段
###### v-model 指令，轻松实现表单输入和应用状态之间的双向绑定

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


## Vue - MVVM 模式
* MVP 模式 - 面向 DOM 进行开发

    *Model(数据层)、View(视图层/DOM展示)、Presenter(业务逻辑/控制层)*

    [TodoList Jquery](./practices/TodoList_Jquery.html)
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
    </head>
    <body>
    <!--视图-->
    <div id="app">
        <input id="input" type="text" >
        <button id="btn">提交</button>
        <ul id="list">
            <li>item 1</li>
        </ul>
    </div>
    <script>
        // MVP模式： M 模型层（Ajax请求） V 视图 P 控制器
        function Page() {

        }

        $.extend(Page.prototype, {
            init: function() {
                this.bindEvents()
            },
            bindEvents: function() {
                var btn = $("#btn");
                btn.on("click", $.proxy(this.handleBtnClick, this))
            },
            handleBtnClick: function() {
                //alert("123");
                var inputElem = $("#input");
                var inputValue = inputElem.val();
                var ulElem = $("#list");
                ulElem.append("<li>" + inputValue + "</li>");
                inputElem.val("");
            }
        })

        var page = new Page();
        page.init();
    </script>
    </body>
    </html>
    ```
    *$ 是 JQuery 常用的一个回传函数，定义为 "选取". 英文是 selector 的缩写*

    <div align="center">
    <b><i>MVP 模式中，Presenter（控制） 层是编码核心，Model层为边缘层</i></b> <br>
    <img src="./pics/MVP.jpg" width="45%">
    </div>

* MVVM 模式 - 面向数据进行编程

    [TodoList Vue](./practices/TodoList.html)
    ```html
    # refer to TodoList Project
    ```

    <div align="center">
    <b><i>MVVM 模式中, VM 层为 Vue 内置，编码重点一部分在视图层，一部分在模型层</i></b> <br>
    <img src="./pics/MVVM.jpg" width="45%">
    </div>


## Vue - 前端组件化
*以组件组合方式搭建页面*

###### 组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树。

#### 1. 使用组件改造 TodoList 网页

* 组件定义 （全局/局部方式）
* v-bind
* props

###### v-bind 指令将待办项传到循环输出的每个组件中
###### 子单元通过 prop 接口与父单元进行良好的解耦

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>TodoList</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="root">
    <div>
        <input type="text" v-model="todoValue">
        <button @click="handleBtnClick">提交</button>
    </div>
    <ul>
        <!--<li v-for="item in list">{{item}}</li>-->
        <todo-item v-bind:content="item"
                   v-for="item in list">
        </todo-item>
    </ul>
</div>
<script>
        // Vue 组件定义 (绑定传参)
        // 全局组件
        //Vue.component("TodoItem", {
        //    props: ["content"],
        //    template: "<li>{{content}}</li>"
        //})
        // 局部组件
        var TodoItem = {
            props: ["content"],
            template: "<li>{{content}}</li>"
        }

        var app = new Vue({
            el: "#root",
            components: {
				// 局部组件注册
                TodoItem: TodoItem
            },
            data: {
                list: ["item 1", "item 2"],
                todoValue: ""
            },
            methods: {
                handleBtnClick: function() {
                    this.list.push(this.todoValue);
                    this.todoValue = "";
                }
            }
        })
</script>
</body>
</html>
```
code link: [practices/TodoList_Vue_Component.html](./practices/TodoList_Vue_Component.html)

#### 2. 组件间传值

***父组件 - Vue 实例绑定的元素 <br>***
***子组件 - TodoItem***

* 父组件 -> 子组件传值
    ```
    # v-bind (简写为:) + props
    ```
* 子组件 -> 父组件传值
    ```
    # v-on （监听事件，简写为@） + $emit （发射/引发事件 + 参数）
    ```
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>TodoList</title>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    </head>
    <body>
    <div id="root">
        <div>
            <input type="text" v-model="todoValue">
            <button @click="handleBtnClick">提交</button>
        </div>
        <ul>
            <!--<li v-for="item in list">{{item}}</li>-->
            <todo-item v-bind:content="item"
                       v-bind:index="index"
                       v-for="(item,index) in list"
                       @delete="handleItemDelete">
            </todo-item>
        </ul>
    </div>
    <script>
            // Vue 组件定义 (绑定传参)
            // 全局组件
            //Vue.component("TodoItem", {
            //    props: ["content"],
            //    template: "<li>{{content}}</li>"
            //})
            // 局部组件
            var TodoItem = {
                props: ["content", "index"],
                template: "<li @click='handleItemClick'>{{content}}</li>",
                methods: {
                    handleItemClick: function() {
                        alert("click");
                        this.$emit("delete", this.index);
                    }
                }
            }

            var app = new Vue({
                el: "#root",
                components: {
                    // 局部组件注册
                    TodoItem: TodoItem
                },
                data: {
                    list: ["item 1", "item 2"],
                    todoValue: ""
                },
                methods: {
                    handleBtnClick: function() {
                        this.list.push(this.todoValue);
                        this.todoValue = "";
                    },
                    handleItemDelete: function(index) {
                        //alert(index);
                        this.list.splice(index, 1);
                    }
                }
            })
    </script>
    </body>
    </html>
    ```
    code link: [practices/TodoList_Vue_Component.html](./practices/TodoList_Vue_Component.html)

##### 总结
至此，简单介绍了 Vue 核心最基本的功能。请详细阅读教程 https://cn.vuejs.org/v2/guide 的介绍部分。

**A video tutorial**: https://scrimba.com/playlist/pXKqta

###### 在一个大型应用中，有必要将整个应用程序划分为组件，以使开发更易管理。不过这里有一个 (假想的) 例子，以展示使用了组件的应用模板是什么样的：
```html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```


##### For advanced Contents, please see other notes.