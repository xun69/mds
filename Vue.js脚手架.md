# Vue.js脚手架

> Vue 提供了一个[官方的 CLI](https://github.com/vuejs/vue-cli)，为单页面应用 (SPA) 快速搭建繁杂的脚手架。它为现代前端工作流提供了 batteries-included 的构建设置。只需要几分钟的时间就可以运行起来并带有热重载、保存时 lint 校验，以及生产环境可用的构建版本。更多详情可查阅 [Vue CLI 的文档](https://cli.vuejs.org/)。

## 1.安装Node.js与WebPack

Vue CLI基于Node.js的npm安装，而且后续会用到WebPack功能。

### 安装Node.js

此处省略。

### npm全局安装vue-cli

```shell
npm install --global vue-cli
```

或者

```shell
cnpm install --global vue-cli
```

![1559407885970](U:\Vue.js脚手架.assets\1559407885970.png)

#### 测试vue-cli是否安装成功

有两种方法测试vue-cli是否安装成功

```shell
vue --vesion
```

或

```shell
vue
```

![1559408292312](U:\Vue.js脚手架.assets\1559408292312.png)

![1559410573694](U:\Vue.js脚手架.assets\1559410573694.png)

### 安装Webpack

```shell
npm install -g webpack
```

## 2.下载模板项目

所谓的Vue.js脚手架的意思其实就是用命令行工具和命令快速下载一个Vue模板项目，然后通过这个模板项目展开开发工作。

### 1.创建项目文件夹

首先需要创建一个项目文件夹来作为模板项目的容器。可以手动创建或在cmd中直接创建。

### 2.在项目文件夹下下载模板项目

当前目录下快速进入cmd 的一种方法：

![1559410507547](U:\Vue.js脚手架.assets\1559410507547.png)

![1559410524699](U:\Vue.js脚手架.assets\1559410524699.png)

#### 下载模板项目

```shell
vue init webpack vue_demo
```

回车后显示“download template”,下载完毕后显示如下：

![1559411303144](U:\Vue.js脚手架.assets\1559411303144.png)

经过一系列的配置后就可以第一次打包运行模板项目了

```shell
npm run dev
```

正常打包后打开http://localhost:8080/将看到如下显示结果：

![1559412218508](U:\Vue.js脚手架.assets\1559412218508.png)

## 3.操作过程流程总结

![vue脚手架-01](U:\Vue.js脚手架.assets\vue脚手架-01.png)

![1559414355309](U:\Vue.js脚手架.assets\1559414355309.png)