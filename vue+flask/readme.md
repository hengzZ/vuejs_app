# 使用 Vue.js 和 Flask 实现全栈单页面应用
组成：
* 前端: vue.js 单页面应用
* 后端: flask
* api 接口

冲突: 
* 与 vue.js 一样，Jinji2（模板引擎）也使用双大括号来渲染页面! <br>
* [flask-vuejs 项目](https://github.com/yymm/flask-vuejs) 解决了 vue 与 flask 的语法冲突.

目标：
* Flask 运行的服务可以访问 index.html 首页和 Vue.js 应用
* 在前端开发环境，使用 Webpack 和它提供的很多非常棒的功能
* 可以从前端的单页面应用访问 Flask 的 API 接口
* 以 Node.js 服务运行的前端开发环境同样也可以访问 API 接口


## 环境搭建

#### 1. npm (nodejs) 包管理器安装
```bash
https://nodejs.org/ (windows 系统安装包)
```

#### 2. vue-cli 全局安装
vue-cli 是 vue.js 的脚手架，用于自动生成 vue.js+webpack 的项目模板，分为 vue init webpack-simple \[项目名] 和 vue init webpack \[项目名] 两种。
```bash
# 设置代理
npm config set https-proxy http://server:port
npm config set proxy http://server:port
npm set registry http://registry.npm.taobao.org
# 安装
npm install -g vue-cli
```

#### 3. 创建项目模板
前端部分：
```bash
mkdir flaskvue
cd flaskvue 
vue init webpack frontend
```
配置项：
* Vue build — Runtime only （Vue 构建的版本 - 运行时）
* Install vue-router? — Yes （安装 vue-router？ - 是）
* Use ESLint to lint your code? — Yes （使用 ESLint 校验你的代码？ - 是）
* Pick an ESLint preset — Standard （选择 ESList 的预置版本 - 标准）
* Setup unit tests with Karma + Mocha? — No （使用 Karma + Mocha 设置单元测试？ - 否）
* Setup e2e tests with Nightwatch? — No （使用 Nightwatch 设置端到端测试？ - 否）

下一步：
```bash
cd frontend
npm install
# 启动
npm run dev
```


## vue 页面搭建

#### 1. 添加 Home.vue 和 About.vue 到 frontend/src/components 文件夹：
* Home.vue
```html
<template>
  <div>
    <p>Home page</p>
  </div>
</template>
```
* About.vue
```html
<template>
  <div>
    <p>About</p>
  </div>
</template>
```

#### 2. 修改 frontend/src/router/index.js 文件来一个个渲染我们的新组件：
```html
import Vue from 'vue'
import Router from 'vue-router'

const routerOptions = [
  { path: '/', component: 'Home' },
  { path: '/about', component: 'About' }
]

const routes = routerOptions.map(route => {
  return {
    ...route,
    component: () => import(`@/components/${route.component}.vue`)
  }
})

Vue.use(Router)

export default new Router({
  routes,
  mode: 'history'
})
```

#### 3. 修改静态资源输出路径
修改 frontend/config/index.js 文件内：
```html
index: path.resolve(__dirname, '../dist/index.html'),
assetsRoot: path.resolve(__dirname, '../dist'),
```
改成如下内容
```html
index: path.resolve(__dirname, '../../dist/index.html'),
assetsRoot: path.resolve(__dirname, '../../dist'),
```
此时，包含 html/css/js 静态资源包的 /dist 文件夹和 /frontend 在同一级目录下.

#### 4. 构建项目
```bash
npm run build
```


## Flask 后端搭建

1. 环境配置：
```bash
mkdir backend
cd backend
virtualenv -p python3 --no-site-packages venv
# 激活虚拟化环境
source venv/bin/activate
# 安装 flask
pip install Flask
```

#### 2. 后端代码
```bash
touch run.py
```
* run.py
```python
from flask import Flask, render_template

app = Flask(__name__,
            static_folder = "../dist/static",
            template_folder = "../dist")

@app.route('/')
def index():
    return render_template("index.html")


if __name__ == "__main__":
    app.debug = True
    app.run()
```

#### 3. 运行
```bash
python run.py
```

#### 4. 修改 Flask 重定向
此时，/about 页面不存在，我们现在重定向所有的路由都跳转到 index.html 上
```python
from flask import Flask, render_template

app = Flask(__name__,
            static_folder = "../dist/static",
            template_folder = "../dist")


@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')
def catch_all(path):
    return render_template("index.html")


if __name__ == "__main__":
    app.debug = True
    app.run()
```


## 添加 404 页面
在 frontend/src/router/index.js 增加一行：
```html
const routerOptions = [
  { path: '/', component: 'Home' },
  { path: '/about', component: 'About' },
  { path: '*', component: 'NotFound' }
]
```
通配符 '*' 在 vue-router 里的含义是以上路由定义之外的情况。现在我们需要在 /components 文件夹新建 NotFound.vue 文件:

* NotFound.vue
```html
<template>
  <div>
    <p>404 - Not Found</p>
  </div>
</template>
```
现在 通过 npm run dev 重新启动前台服务然后随意输入网址像 localhost:8080/gljhewrgoh。你应该看到 “Not Found” 两个单词。


## 添加后端 API 接口
现在，我们将在后端创建一个 API 接口然后通过前端来调用它。

#### 1. 创建一个随机返回数字1到100的简单接口
打开 run.py 新增如下代码：
```python
from flask import Flask, render_template, jsonify
from random import *

app = Flask(__name__,
            static_folder = "../dist/static",
            template_folder = "../dist")

@app.route('/api/random')
def random_number():
    response = {
        'randomNumber': randint(1, 100)
    }
    return jsonify(response)

@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')
def catch_all(path):
    return render_template("index.html")


if __name__ == "__main__":
    app.debug = True
    app.run()
```
以上代码首先导入 random 库和 jsonify 函数。然后增加一个**返回 JSON 数据格式的新路由** /api/random。

json 数据格式如下：
```python
{
  "randomNumber": 36
}
```

#### 2. 在前端调用 API 接口
修改 Home.vue 组件：
```html
<template>
  <div>
    <p>Home page</p>
    <p>Random number from backend: {{ randomNumber }}</p>
    <button @click="getRandom">New random number</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      randomNumber: 0
    }
  },
  methods: {
    getRandomInt (min, max) {
      min = Math.ceil(min)
      max = Math.floor(max)
      return Math.floor(Math.random() * (max - min + 1)) + min
    },
    getRandom () {
      this.randomNumber = this.getRandomInt(1, 100)
    }
  },
  created () {
    this.getRandom()
  }
}
</script>
```
详解：
* 初始变量 randomNumber 等于 0
* 在 methods 部分，我们用 getRandomInt(min, max) 函数从指定区间返回一个数字， getRandom 函数将调用上一个函数生成一个值赋给 randomNumber
* 在组件被创建时调用 getRandom 方法给 randomNumber 赋个初始数值
* 在按钮点击事件里，我们将触发 getRandom 方法去得到一个数值


## 用 axios 库来连接后端
axios 将允许我们创建能返回 Promise 对象的 HTTP 请求.

#### 1. axios 安装
```bash
npm install --save axios
```

#### 2. vuejs 调用 axios
再次打开 Home.uve，修改 **\<script>** 部分代码：
```html

```


##### referencd
[1] https://github.com/oleg-agapov/flask-vue-spa