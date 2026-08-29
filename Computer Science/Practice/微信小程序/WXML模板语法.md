# 数据绑定

## 基本原则

1. 在data中定义数据
    
    在页面对应的 .js 文件中，把数据定义到 data 对象中即可
    
    ```JavaScript
    Page({
      data: {
          // 字符串类型的数据
          info: 'init data',
          // 数组类型的数据
          msgList: [{msg: 'hello'}, {msg: 'world'}]
      }
     }）
    ```
    
    - Mustache语法：把 data 中的数据绑定到页面中渲染（双大括号）
    
    ```HTML
    <view>{{ 要绑定的数据类型 }}</view>
    ```
    
    应用场景：  
    ① 绑定内容  
    
    Eg. 动态绑定内容
    
    ```JavaScript
    Page({
      data: {
            // 字符串类型的数据
            info: 'init data'
      }
     }）
    ```
    
    ```HTML
    <view>{{ info }}</view>
    ```
    
    ![[image 22.png|image 22.png]]
    
    ② 绑定属性
    
    ```JavaScript
    Page({
      data: {
            // 动态绑定属性
            imgScr: '/images/tinyHerb.jpg'
      }
     }）
    ```
    
    ```HTML
    <image src="{{imgScr}}"></image>
    ```
    
    ③ 运算（三元运算、算术运算等）
    
    1. 三元运算
        
        ```JavaScript
        Page({
          data: {
                // 三元运算
                randomNum: Math.random() * 10 // 生成10以内的随机数
          }
         }）
        ```
        
        ```HTML
        <view>{{ randomNum >= 5 ? '随机数字大于或等于5' : '随机数字小于5' }}</view>
        ```
        
    2. 算数运算
        
        ```JavaScript
        Page({
          data: {
                // 算数运算
                randomNum: Math.random().toFixed(2) // 生成一个带两位小数的随机数，如0.34
          }
         }）
        ```
        
        ```HTML
        <view>生成100以内的随机数{{ randomNum * 100 }}</view>
        ```
        
2. 在WXML中使用数据

# 事件绑定

## 定义

事件是渲染层的通讯方式。通过事件可以将用户在渲染层产生的行为，反馈到逻辑层进行业务的处理。

## 常用事件

|类型|绑定方式|事件描述|
|---|---|---|
|tap|bindtap 或 bind:tap|手指触摸后马上离开，类似于HTML中的click事件|
|input|bindinput 或 bind:input|文本框的输入事件|
|change|bindchange 或 bind:change|状态改变时触发|

## 事件对象的属性列表

当事件回调触发时，会收到一个事件对象event，其详细属性如下表所示。

|属性|类型|说明|
|---|---|---|
|type|String|事件类型|
|timeStamp|Integer|页面打开到触发事件所经过的毫秒数|
|target|Object|触发事件的组件的一些属性值集合|
|currentTarget|Object|当前组件的一些属性值集合|
|detail|Object|额外的信息|
|touches|Array|触发事件，当前停留在屏幕中的触摸点信息的数组|
|changeTouches|Array|触摸事件，当前变化的触摸点信息的数组|

## target和currentTarget的区别

### target

触发事件的源头组件

### currentTarget

当前事件所绑定的组件

![[IMG_2442(20250223-220404).jpg]]

## bindtap的语法格式

在微信小程序中，不存在HTML中的onclick鼠标点击事件，而是通过 tap事件来响应用户的触摸行为。

1. 通过 bindtap，可以为组件绑定 tap触摸事件。
    
    ```HTML
    <button type="primary" bind:tap="btnTapHandler">按钮</button>
    ```
    
2. 在页面的 .js 文件中定义对应的事件处理函数，事件参数通过形参 event（一般简写成 e ）来接收。
    
    ```JavaScript
    Page({
    	// 定义按钮的tap事件处理函数
      btnTapHandler(e) { 
          console.log(e) // 事件参数对象e
      }
     }）
    ```
    

## 在事件处理函数中为data中的数据赋值

通过调用 this.setData(dataObject) 方法，可以给页面 data 中的数据重新赋值。

```JavaScript
Page({
	data: {
    count: 0
  },

  // 修改 count 的值
  changeCount() {
    this.setData({
        count: this.data.count + 1
    })
  }
 }）
```

```HTML
<button type="primary" bind:tap="changeCount">+1</button>
```

## 事件传参

小程序中的事件传参比较特殊，不能在绑定事件的同时为事件处理函数传递参数。

可以为组件提供 data-* 自定义属性传参，其中 *** 代表的是参数的名字**。

```HTML
<!-- 2为数字  -->
<button type="primary" bind:tap="btnHandler" data-info="{{2}}">+2按钮</button>

<!-- 2为文本  -->
<button type="primary" bind:tap="btnHandler" data-info="2">+2按钮</button>
```

最终，

- info 会被解析为**参数的名字**
- 数值 2 会被解析为**参数的值**

在事件处理函数中，通过every.target.dataet.参数名 即可获取到具体参数的值。

```JavaScript
Page({
	btnHandler(e) {
    // dataset是一个对象，包含了所有通过data-*传递过来的参数
    console.log(e.target.dataset)
    // 通过dataset可以访问到具体参数的值
    console.log(e.target.dataset.info)
  }
 }）
```