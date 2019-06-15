# VS code 整合JsDoc

JsDoc是一种通过让程序员对JS代码进行预定规则的规范化注释，然后通过命令行生成HTML API文档集合。

VScode对jsDoc的插件支持，让VScode可以通过jsDoc格式的带am注释来实现语法提示等功能，进一步加强VScode的使用效能。

优点：

1. 规范了代码的注释，让代码更易于阅读；
2. 增强VScode代码提示，让大项目代码编写更容易；
3. 快速生成API文档，便于查询；

## VS code编辑器注释与取消注释快捷键

单行注释：`Ctrl + /`

块注释：`Alt + Shift + A`

## 安装Js doc实现代码注释的格式化与代码提示

![1555226233617](U:\VS code编辑器.assets\1555226233617.png)

![1555226373156](U:\VS code编辑器.assets\1555226373156.png)

![1555226398158](U:\VS code编辑器.assets\1555226398158.png)

![1555226513606](U:\VS code编辑器.assets\1555226513606.png)

![1555226530304](U:\VS code编辑器.assets\1555226530304.png)

![1555226804657](U:\VS code编辑器.assets\1555226804657.png)



![1555227238402](U:\VS code编辑器.assets\1555227238402.png)

## 整合Nodejs

### 下载安装nodejs

![1555227662632](U:\VS code编辑器.assets\1555227662632.png)

### 在VS code终端（命令行）中测试和使用nodejs

nodejs安装后，可以直接在VScode自带的命令行（终端）中进行测试和使用；

nodejs安装后需要重启一下VScode才能正常使用nodeJS。

![1555228011790](U:\VS code编辑器.assets\1555228011790.png)

## 使用npm安装jsDoc

使用`npm install jsdoc -g`命令安装jsDoc。

```powershell
U:\codes>npm install jsdoc -g
C:\Users\Administrator\AppData\Roaming\npm\jsdoc -> C:\Users\Administrator\AppData\Roaming\npm\node_modules\jsdoc\jsdoc.js
jsdoc@3.5.5 C:\Users\Administrator\AppData\Roaming\npm\node_modules\jsdoc
├── taffydb@2.6.2
├── escape-string-regexp@1.0.5
├── strip-json-comments@2.0.1
├── underscore@1.8.3
├── babylon@7.0.0-beta.19
├── marked@0.3.19
├── bluebird@3.5.4
├── requizzle@0.2.1 (underscore@1.6.0)
├── js2xmlparser@3.0.0 (xmlcreate@1.0.2)
├── klaw@2.0.0 (graceful-fs@4.1.15)
├── mkdirp@0.5.1 (minimist@0.0.8)
└── catharsis@0.8.9 (underscore-contrib@0.3.0)

U:\codes>
```

## 生成API文档

命令行输入`jsdoc 1.js`命令回车，执行API文档生成；

![1555228513727](U:\VS code编辑器.assets\1555228513727.png)

![1555228624463](U:\VS code编辑器.assets\1555228624463.png)

可以看到jsDoc生成的文档样子比较丑，而且很多英文。



Typora图片自动备份文件夹设置

![1555225063173](U:\VS code编辑器.assets\1555225063173.png)

