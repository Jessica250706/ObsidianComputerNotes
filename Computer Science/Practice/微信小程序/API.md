# 概述

小程序中的API由宿主环境提供，通过这些丰富的小程序API，开发者可方便地调用微信提供的能力，如获取用户信息、本地存储、支付功能等。

# 分类

## 事件监听API

### 特点

以 **on** 开头，用于**监听某些事件的触发**

### 举例

wx.**onWindowResize**(function callback) 监听窗口尺寸变化的事件

## 同步API

### 特点

1. 以 **Sync** 结尾
2. 同步API的执行结果，可通过函数返回值直接获取，若执行出错会抛出异常

### 举例

wx.**setStorageSync**(’key’, ‘value’) 向本地存储中写入内容

## 异步API

### 特点

类似于 jQuery 中的 **$.ajax(options)** 函数，需要通过 success、fail、complete 接收调用的结果

### 举例

wx.**request**() 发起网络数据请求，通过 success 回调函数接收数据