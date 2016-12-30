---
layout: post
title: "npm install node_modules bugs fixed"
date: 2016-12-30 16:39:28 +0800
categories: node npm
---

最近在windows下安装一个依赖极多的前端项目时遇到了各种bug，记录下：
* 1. node-sass无法生成vendor文件夹
* 2. ngreact注释使用了react 12以前的语法风格 `/** @jsx React.DOM */` ，build报错
* 3. react-dropzone被二次依赖，但需要babel来build，node_modules下 `.babelrc` 报错

解决方案：
* 1. `npm rebuild node-sass`
* 2. 查看node_modules下源码。删除注释
* 3. 进入 `C:\Users\......\node_modules\.3.8.0@react-dropzone` 目录，执行 `npm install` 安装该模块build依赖
