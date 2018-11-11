## 5. webpack

官网: https://webpack.docschina.org/

> 前端工程化工具 webpack
> webpack主要使用场景是单页面富应用（ SPA )    

### 5.1 前端自动化（半自动化〉工程主要解决以下问题：

- JavaScript 、 css 代码的合井和压缩 。
- css 预处理 ： Less 、 Sass 、 Stylus 的编译。
- 生成雪碧图（ CSS Sprite ）。
- ES 6 转 ES 5.
- 模块化。

归根到底， webpack 就是一个.js 配置文件，你的架构好或差都体现在这个配置里，随着需求 的不断出现，工程配置也是逐渐完善的    

### 5.2 webpack的核心

​	默认只能打包 JavaScript
	要解析其他文件 需要安装 对应的 loader配置规则即可

### 5.3 安装loader(加载器)解析不同资源

npm install css-loader --save-dev    

在 webpack 的世界里，每个文件都是一个模块，比如css 、 .js 、 .h恤l、 .jpg、 .less 等 。 对于不 同的模块，需要用不同的加载器（ Loaders ）来处理，而加载器就是 webpack 最重要的功能 。    

### 5.4 单文件组件与 vue-loader 的基本结构

vue-loader 在编译 .vue 文件时， 会对＜template＞、＜scrip t> 、＜style＞分别处理，所以在 vue-loader 选项里多了 一项 options 来进一步对不同语言进行配置。    

```html
<template> 

</template> 

<script> 

</script> 

<style>

</style>

```

