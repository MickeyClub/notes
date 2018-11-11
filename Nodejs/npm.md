## npm的使用

[TOC]



### 1. npm

使用`npm init`或者`npm init -y`初始化项目目录，生成package.json配置文件

`npm install 包名` 下载该包的最新版本   `npm install jquery`

`npm install 包名@版本号 ` 下载指定版本的包  `npm install jquery@1.11.1`

`npm info 包名` 查询指定包的信息

`npm uninstall 包名`   删除指定包  

`npm install 包名 -g` 全局安装，只有支持全局安装的包才可以以这样的方式安装.

`--save` 和 `--save-dev`

`npm install --production` 

`npm install`



**package.json**

- scripts
- dependencies
- devDependencies

###  2. cnpm

不建议使用

```
npm install cnpm -p
淘宝 NPM 镜像
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



### 3. 指定npm下载源

**建议一次性配置,让每次下载都去淘宝服务器下载,防止下包失败**

**修改配置文件**,指向淘宝服务器

```
npm config set registry http://registry.npmjs.org    
//设置npm下载源指定为淘宝服务器
npm config set registry https://registry.npm.taobao.org 
```

**命令行指定下载(vue)**

```
npm install vue --registry http://registry.npm.taobao.org
```



### 4. yarn