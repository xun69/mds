# npm学习

![img](https://www.npmjs.cn/images/npm.svg)

## 教程

- [npm火速上手 - 表严肃讲npm](https://www.bilibili.com/video/av13816480/?p=3)

这几天之前依赖BootCDN引入方式搭建的几个小工具都不工作了。突然发现CDN引入的方式有时候真的不可靠啊。

所以突然想起来NPM方式。

而且关于Vue的进阶学习——也就是基于ES6语法和.vue文件——也是一件很重要的事情。

## npm网站

- [npm官网](https://www.npmjs.com/)
- [npm中文文档](https://www.npmjs.cn/)

npm主要包括命令行和网站两部分，所有的包极其版本都发布在npm网站上，可以进入网站查询相应的包，并且都提供README来简单介绍包和解释如何安装等。

![1560513950842](U:\[markdown]\NPM学习.assets\1560513950842.png)

# WebPack学习

- [玩转Webpack - 表严肃讲webpack](https://www.bilibili.com/video/av14235184/?p=2)
- [webpack中文文档](https://www.webpackjs.com/concepts/)

解决多个库引入时可能存在的全局变量冲突隐患。

在node的眼里所有的文件都是一个模块，有一个入水口一个出水口。

b.js

```js
var msg = "aaa";
module.exports = {msg:msg};//暴露b.js中的变量msg
```

a.js

```js
var msg = require("./b").msg;//引入b.js中暴露的msg
console.log(msg);
```

上面的两个js采用的是node.js的后端语法，在前端是无法直接运行的。直接运行会在浏览器报错：

![1560516084902](U:\[markdown]\NPM学习.assets\1560516084902.png)

正确运行方法：

```shell
node a.js
```

node解释执行。

或者用webpack打包为前端可用的js文件：

```shell
webpack a.js -o bundle.js
```

打包后可以直接在浏览器运行。

## 安装

在项目目录下首先初始化npm

```shell
npm init -y
```

生成package.json文件后，webpack就会认为整个目录下是一个模块。

### 全局安装webpack和webpack-cli

webpack3.0及其以上或4.0以下版本自带webpack-cli，不需要额外安装。而4.0以上则需要。

```shell
npm i webpack -g
```

- i:install的缩写；
- -g:表示global全局安装；

```shell
npm i webpack-cli -g
```

# Git使用

![1560523255316](U:\[markdown]\NPM学习.assets\1560523255316.png)

安装成功：

![1560523362354](U:\[markdown]\NPM学习.assets\1560523362354.png)

## 设定身份信息

```shell
git config --global user.name "WendyStar"
git config --global user.email "1308435433@qq.com"
```

## 创建工作区目录

可以手动在本地创建工作区目录，也就是项目文件夹。

## 创建git仓库

```shell
git init
```

上面的命令执行后将会在工作区目录下创建名叫".git"的隐藏目录，它就是当前项目的本地git仓库。

## git 与远程仓库

### 添加远程仓库

```shell
git remote add test https://github.com/xun69/vscodeGitTest
```

### 查看已有的远程仓库

```shell
$ git remote -v
test    https://github.com/xun69/vscodeGitTest (fetch)
test    https://github.com/xun69/vscodeGitTest (push)
```

### push

```shell
git push -u test master
```

在远程push之前，需要先执行本地的add和commit操作

```shell
git add index.html
git commit -m "创建index.html"
```

![1560579621647](U:\[markdown]\NPM学习.assets\1560579621647.png)

push 命令执行后会弹出github登录对话框，登陆后就会进行push操作。