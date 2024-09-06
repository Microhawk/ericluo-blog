---
title: 用patch-package给依赖打补丁
date: 2022-05-10
description: 用patch-package给node_module中的包打补丁
category: JavaScript
---

## 用patch-package给node_module中的包打补丁

问题描述：在项目中用到一个第三方库，但这个库有个bug并且已经影响到了你的项目，需要修改这个库的源码才能解决。

一般的解决方案：

- 方案一：提issue或者pr，联系作者修改；
- 方案二：下载该库的源码到本地 ，放在src目录，修改后手动引入
- 方案三：fork该库的代码到自己仓库，修改后，从自己仓库安装这个插件

这三种方案都比较暴力以及繁琐，都不是最优解。最优解是通过`patch-package`打补丁。

## 1.安装patch-package

```shell
npm install patch-package --save-dev
或者
yarn add patch-package postinstall-postinstall
(npm安装不需要postinstall-postinstall依赖)
```

![](https://cdn.jsdelivr.net/gh/microhawk/image-host/20240906181335.png)

## 2.修改本地项目的package.json文件，增加命令

```json
 "postinstall": "patch-package"
```

![](https://cdn.jsdelivr.net/gh/microhawk/image-host/20240906181538.png)

## 3.到node_modules中找到对应的库，并修改源码

## 4.手动执行命令，创建补丁文件

```shell
// 创建补丁文件
npx patch-package package-name
```

这时候会在项目根目录下多出一个文件夹：`patches`

patches文件夹下的文件是以`package-name + version`命名的文件。

删除node_module，然后重新安装依赖，检查补丁是否生效。

:::warning

patch是锁定版本号的，如果升级了版本，patch内容将会失效，所以要锁定package.json中的版本号。

yarn包管理器有yarn.lock文件锁定依赖版本。[锁定依赖](https://www.codetd.com/article/7624217)

:::

