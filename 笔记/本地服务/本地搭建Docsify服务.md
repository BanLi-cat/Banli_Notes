## 执行下载 Node.js

```sh
# Node.js 官网下载页面
https://nodejs.org/en/download/
```

## 安装 docsfiy-cli

```sh
# 安装 docsify-cli 后就可以在任何位置使用 docsify 命令调用 docsify.cmd 程序，然后会执行这个路径的 docsify 文件
npm i docsify-cli -g
```

## 初始化项目

```sh
# 初始化一个项目，项目文件夹名称叫 docs。

docsify init ./docs

# .nojekyll 用于阻止 GitHub Pages 忽略掉下划线开头的文件
# index.html 入口文件
# README.md 会作为主页内容渲染
```

## 运行项目

```sh
docsify serve .

# 运行成功
# Serving E:\OnlineNotes now.
# Listening at http://localhost:3000
```
