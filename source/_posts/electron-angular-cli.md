---
title: 使用electron构建angular桌面应用程序
tags:
  - electron
  - angular
categories:
  - electron
abbrlink: 2268864848
date: 2018-04-12 15:53:44
---
使用electron打包angular为桌面应用程序
<!-- more -->
### 创建angular项目
如果没安装angular-cli，先安装angular-cli
``` 
npm install -g @angular/cli
```
使用ng创建项目
```
ng new electron-angular-demo
```
安装依赖
``` 
npm install
```
使用IDE工具打开 `electron-angular-demo`

更新 `src/index.html`
``` 
<base href="./">
```
### 配置electron
安装electron
``` 
npm install electron --save-dev
```
在项目根目录下创建 `main.js`

```javascript
const {app, BrowserWindow} = require('electron')
const path = require('path')
const url = require('url')

// 保持一个对于 window 对象的全局引用，如果你不这样做，
// 当 JavaScript 对象被垃圾回收， window 会被自动地关闭
let win

function createWindow () {
  // 创建浏览器窗口。
  win = new BrowserWindow({
    width: 800,
    height: 600,
    // icon: `file://${__dirname}/dist/assets/logo.png`
  })

  // 然后加载应用的 index.html。
  win.loadURL(`file://${__dirname}/dist/index.html`)

  // 打开开发者工具。
  win.webContents.openDevTools()

  // 当 window 被关闭，这个事件会被触发。
  win.on('closed', () => {
    // 取消引用 window 对象，如果你的应用支持多窗口的话，
    // 通常会把多个 window 对象存放在一个数组里面，
    // 与此同时，你应该删除相应的元素。
    win = null
  })
}

// Electron 会在初始化后并准备
// 创建浏览器窗口时，调用这个函数。
// 部分 API 在 ready 事件触发后才能使用。
app.on('ready', createWindow)

// 当全部窗口关闭时退出。
app.on('window-all-closed', () => {
  // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
  // 否则绝大部分应用及其菜单栏会保持激活。
  if (process.platform !== 'darwin') {
  app.quit()
}
})

app.on('activate', () => {
  // 在macOS上，当单击dock图标并且没有其他窗口打开时，
  // 通常在应用程序中重新创建一个窗口。
  if (win === null) {
  createWindow()
}
})

// 在这文件，你可以续写应用剩下主进程代码。
// 也可以拆分成几个文件，然后用 require 导入。

```
更新 `package.json`
``` 
{
  "name": "electron-angular-demo",
  "version": "0.0.0",
  "license": "MIT",
  "main": "main.js", // 添加
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e",
    "electron": "electron .", // 添加
    "electron-build": "ng build --prod && electron ." // 添加
  },
  // ...
}
```


### 运行electron
```  
npm run electron-build
```
效果截图
![效果截图](electron-angular-cli\23627f60.png)

如果出现 `Error: Cannot find module '@angular-devkit/core'`，检查 `package.json` 的 `@angular/cli`
``` 
// "@angular/cli": "1.5.5" 换成
"@angular/cli": "^1.5.5"
```
在运行
``` 
npm update
```