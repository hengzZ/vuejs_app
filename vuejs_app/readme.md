## Vue App

#### Part I. 环境部署

##### 1. Node.js 运行环境
```
https://nodejs.org 下载 LTS 版本
```
##### 2. vue-cli 工程搭建助手
```
# 全局安装 vue-cli
npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
vue init webpack my-project
# 安装依赖， 走你
cp my-project
npm install 
npm run dev
此时，你已拥有一个 Vue 的 demo 项目实例
```
##### 3. webpack, 前端目前最流行的编译打包工具


#### Part II. 项目架构

webpack 项目结构

```
▪ index.html (首页)
▪ main.js (入口)
▪ App.vue (单文件根组件)
▪ router (路由)
▪ @/components （单文件子组件）

以上为主线逻辑。
```

API 请求

```html
axios 进行 ajax 请求，获取 json 数据
Ajax 即 “Asynchronous Javascript And XML” （异步 JavaScript 和 XML）
npm install axios --save

可使用 webpack 的 dev server 代理机制从本地获取模拟 json 文件，进行前端开发！！
```
