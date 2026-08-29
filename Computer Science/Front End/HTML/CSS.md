---
tags:
  - 前端
  - CSS
---

# 1 CSS 简介

## 1.1 概念

CSS 是 Cascading Style Sheet 的简写，表示层叠样式表，主要用于渲染 HTML 元素在网页中的展示效果。主要包括对元素高度、宽度、字体、颜色、背景图片、边距、定位、呈现方式等设定。

## 1 .2 CSS 选择器分类

- 基本选择器
    - ID 选择器（优先度 max）
    - 类选择器（优先度 medial）
    - 标签选择器（优先度 min）
- 层次选择器

## 1.3 CSS 选择器语法

```CSS
/* 标签选择器 */
标签名{
	声明1;
	声明2;
	...
	声明n;
}

/* 类选择器 */
.类名{
	声明1;
	声明2;
	...
	声明n;
}

/* ID选择器 */
#ID值{
	声明1;
	声明2;
	...
	声明n;
}
```

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>准尘系统登录</title>
    <!-- 非引入 -->
    <style>
        /* 标签选择器：使用标签名来选择元素 */
        div{
            color: cadetblue; /* 文本字体颜色 */
        }
        /* 类选择器 */
        .content{
            color: cornflowerblue;
        }
        /* id选择器 */
        #a{
            color: darkgray;
        }
    </style>
</head>

<body>
    <div id="a" class="content">
        我cp是真的！
    </div>
    <input type="text" class="content">

</body>
</html>
```

## 1.4 CSS 样式引入

CSS 样式分为**行类样式**、**内部样式**和**外部样式**三种。

CSS 样式引入也具有优先级：行内样式 > 内部样式 > 外部样式

### 1.4.1 行类样式

```HTML
<div style="color:red;font-size:20px;">
	这是行内样式
</div>
```

### 1.4.2 内部样式

```HTML
<style>
	#demo{
		color:red;
		font-size:20px;
	}
</style>

<div id="demo">
	这是内部样式
</div>
```

### 1.4.3 外部样式

```CSS title:demo.css
#demo {
	color:red;
	font-size:20px;
}
```

```HTML title:demo.html
<head>
	<link type="text/css" href="demo.css" rel="stylesheet">
</head>
```

```HTML
<!-- 在CSS3中使用 -->
<style>
    @import url(demo.css);
</style>
```

## 1.5 CSS 高级选择器

### 1.5.1 后代选择器

```CSS
div ul li {

}
```

### 1.5.2 子代选择器

```CSS
div > ul > li {

}
```

### 1.5.3 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>青温真好磕</title>  
    <!-- 引入css样式 -->  
    <link href="css/login.css" rel="stylesheet" type="text/css">  
</head>  
  
<body>  
    <div>        
    	<ul>            
    		<li>列表项</li>  
        </ul>        
        <h1>            
        	<ul>                
        		<li>列表项</li>  
            </ul>        
        </h1>    
    </div>  
</body>  
</html>
```

```CSS
/* 后代选择器 */
div  ul  li{  
    color: darkred;  
}  
  
/* 子代选择器 */
div > ul > li{  
    color: forestgreen;  
}
```

![](images/Pasted%20image%2020250505141657.png)

# 2 CSS 样式

## 2.1 字体

| 属性          | 含义              | 举例                             |
| ----------- | --------------- | ------------------------------ |
| font-family | 设置字体类型          | font-family:”楷体”;              |
| font-size   | 设置字体大小          | font-size:12 px;               |
| font-style  | 设置字体风格          | font-style:italic;             |
| font-weight | 设置字体的粗细         | font-weight:bold;              |
| font        | 在一个声明中设置所有的字体属性 | font:italic bold 40 px “微软雅黑”; |

p.s. 字体的复合属性是有顺序的：风格粗细大小类型。

## 2.2 文本

| 属性              | 含义         | 举例                         |
| --------------- | ---------- | -------------------------- |
| color           | 设置文本颜色     | color:#\ #00C               |
| text-align      | 设置元素水平对齐方式 | text-align: right;         |
| text-indent     | 设置首行文本的缩进  | text-indent:20 px;          |
| line-height     | 设置文本的行高    | line-height:25 px;          |
| text-decoration | 设置文本的装饰    | text-decoration:underline; |

```CSS
/* 需要注意：文本两端对齐只对最后一行有效，因此需要添加一个text-align-last属性来完成两端对齐 */
text-align: justify;
text-align-last: justify;
```

### 2.2.1 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>CSS Style</title>  
    <link href="css/style.css" rel="stylesheet" type="text/css">  
</head>  
<body>  
  
    <form action="" method="get">  
        <div>            
        	<label>账号</label>  
            <input type="text">  
        </div>        
        <div>            
        	<label>密码</label>  
            <input type="password">  
        </div>        
        <div>            
        	<label>确认密码</label>  
            <input type="password">  
        </div>    
    </form>  
    
    <p>        
    这是小说的一个段落，截取自《秘密》：从月球回来后，大家都休整了好一会儿。这场战斗太过惨烈，若非最后军团长前来支援，并耗尽生命斩出那一刀，他们必败无疑。之后，溪流锋锐整合了有生力量前往火星寻找贺堂堂，却得知对方被折秋泓接走了。无奈，他们只能配合蔚蓝一起把火星上其余的幸存者带回地球。  
    </p>  
  
    <div id="center">  
        这是文字.  
    </div>  
  
</body>  
</html>
```

```CSS
/* 容器中的元素可以从容器中继承CSS样式 */  
/* 全局 */
html,body {  
    font-family: 幼圆;  
    font-size: 20px;  
    font-style: italic;  
    font-weight: bold;  
}  
  
/* label和span在默认情况下是没有宽度的，不能直接设置宽度，必须将其变成块元素，或者是行内的块元素，才能设置高度和宽度 */
label {  
    display: inline-block; /* display属性：表示圆度的展示方式 */    
    width: 90px;  
    text-align: justify;  
    text-align-last: justify;  
}  
  
p {  
    text-indent: 30px;  
}  
  
#center {  
    /* height和line-height相等时，文字竖直居中 */    
    height: 60px;  
    line-height: 60px;  
    background-color: cadetblue;  
    text-decoration: underline;  
}
```

![](images/Pasted%20image%2020250505153318.png)

## 2.3 背景

| 属性                  | 含义     | 示例                                                    |
| ------------------- | ------ | ----------------------------------------------------- |
| background-color    | 背景颜色   | body{background-color:red;}                           |
| background-image    | 背景图像   | body{background-image:url(图片地址);}                     |
| background-position | 背景定位   | body{background-position:205 px 10 px;}                 |
| background-repeat   | 背景重复方式 | body{background-repeat: no-repeat;}                   |
| background          | 背景属性   | body{background:red url(图片地址) 100 px 100 px no-repeat;} |
### 2.3.1 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>CSS Style</title>  
    <link href="css/style.css" rel="stylesheet" type="text/css">  
</head>  
<body>  
  
    <div id="center">  
        这是微草队徽
    </div>  
  
</body>  
</html>
```

```CSS
#center {  
    /* height和line-height相等时，文字竖直居中 */    
    height: 600px;  
    line-height: 60px;  
    background-color: cadetblue;  
    /* 这里的URL是相对定位，参照物就是CSS文件本身 */    
    background-image: url("../img/微草队徽.jpg");  
    /* 背景图片不重复 */    
    background-repeat: no-repeat;  
    /* 从给定位置开始使用图片 */    
    background-position: 10px 10px;  
}
```

![416](images/Pasted%20image%2020250505154531.png)

## 2.4 边框 + 边距

### 2.4.1 边框

|属性|含义|示例|
|---|---|---|
|border-color|边框颜色|div{border-color:red;}|
|border-width|边框粗细|div{borfrt-width:8 px}|
|border-style|边框样式|div{border-style:solid;}|
|border|边框属性|div{border:8 px solid red;}|

边距分为外边距和内边距。边距有 4 个方向：上、下、左、右。

### 2.4.2 外边距： margin

```CSS
margin-top: 2px;
margin-bottom: 2px;
margin-left: 2px;
margin-right: 2px;
margin: 2px;
```

### 2.4.3 内边距：padding

遵循一个特定的规则，即顺时针方向：上、右、下、左。

```CSS
padding-top: 2px;
padding-bottom: 2px;
padding-left: 2px;
padding-right: 2px;
padding: 2px;
```

### 2.4.4 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>CSS Style</title>  
    <link href="css/test01.css" rel="stylesheet" type="text/css">  
</head>  
<body>  
  
    <div>        
    	你好。  
    </div>  
  
</body>  
</html>
```

```CSS
html,body {  
    width: 100%;  
    height: 100%;  
    margin: 0;  
    padding: 0;  
}  
  
div {  
    height: 100px;  
    border-color: cadetblue;  
    border-style: solid; /* 边框风格 */    
    border-width: 5px; /* 边框粗细 */    
    /* 外边距margin */  
    margin-top: 10px;  
    margin-right: 10px;  
    margin-bottom: 10px;  
    margin-left: 10px;  
    /* 内边距padding */  
    padding-top: 20px;  
    padding-bottom: 20px;  
    padding-left: 20px;  
    padding-right: 20px;  
}
```

![](images/Pasted%20image%2020250505173928.png)

## 2.5 浮动

元素浮动有两个方向：left 和 right。

```CSS
float: left;
```

### 2.5.1 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>CSS Style</title>  
    <link href="css/test01.css" rel="stylesheet" type="text/css">  
</head>  
<body>  
  
    <div>        
    	你好。  
    </div>  
  
    <div class="f1">  
  
    </div>    
    <div class="f2">  
  
    </div>  
</body>  
</html>
```

#### 2.5.1.1 无浮动

CSS 同 2.4.3 中的举例。

![](images/Pasted%20image%2020250505174237.png)

#### 2.5.1.2 有浮动

浮动的元素没有宽度，必须要设置宽度。且该元素是在容器内浮动。

```CSS
html,body {  
    width: 100%;  
    height: 100%;  
    margin: 0;  
    padding: 0;  
}  
  
div {  
    height: 100px;  
    border-color: cadetblue;  
    border-style: solid; /* 边框风格 */    
    border-width: 5px; /* 边框粗细 */    
    /* 外边距margin */  
    margin-top: 10px;  
    margin-right: 10px;  
    margin-bottom: 10px;  
    margin-left: 10px;  
    /* 内边距padding */  
    padding-top: 20px;  
    padding-bottom: 20px;  
    padding-left: 20px;  
    padding-right: 20px;  
}  
  
.f1,  
.f2 {  
    float: left;  
}
```

![](images/Pasted%20image%2020250505174309.png)

```CSS
.f1,  
.f2 {  
    float: left;  
    width: 200px;  
}
```

![](images/Pasted%20image%2020250505174633.png)

## 2.6 清除浮动

清除浮动有三种选择：left、right 和 both。

浮动的元素不占用页面空间，与其他元素不在同一个层级。清除浮动后，浮动的元素就与其他元素在同一个层级了。

```HTML
<style>
	html,body{
		width: 100%;
		height: 100%;
		margin: 0;
		padding: 0;
	}
	.container{
		background: red;
	}
	.block1,
	.block2,
	.block3{
		width:200px;
		height: 100px;
	}
	.block1{
		background: black;
		float: left;
	}
	.block2{
		background: orange;
		float: right;
	}
	.block3{
		background: yellowgreen;
		clear: both
	}
</style>

<div class="container">
	<div class="block1"></div>
	<div class="block2"></div>
	<div class="block3"></div>
</div>
```

### 2.6.1 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>CSS Style</title>  
    <link href="css/test01.css" rel="stylesheet" type="text/css">  
</head>  
<body>  
  
    <div class="s">  
        你好。  
    </div>  
  
    <div class="f1">  
  
    </div>    
    <div class="f2">  
  
    </div>    
    <div class="c"></div>  
    <div class="box">  
        <div class="f1"></div>  
        <div class="f2"></div>  
        <div class="c"></div>  
    </div>
    
</body>  
</html>
```

```CSS
html,body {  
    width: 100%;  
    height: 100%;  
    margin: 0;  
    padding: 0;  
}  
  
.s {  
    height: 100px;  
    border-color: cadetblue;  
    border-style: solid; /* 边框风格 */    
    border-width: 5px; /* 边框粗细 */    
    /* 外边距margin */  
    margin-top: 10px;  
    margin-right: 10px;  
    margin-bottom: 10px;  
    margin-left: 10px;  
    /* 内边距padding */  
    padding-top: 20px;  
    padding-bottom: 20px;  
    padding-left: 20px;  
    padding-right: 20px;  
}  
  
.f1,  
.f2 {  
    float: left;  
    width: 200px;  
    height: 300px;  
    border: 5px solid lightsteelblue;  
}  
  
.box {  
    height: 200px;  
    background-color: darkseagreen;  
}  
  
.c {  
    clear: both;  
}
```

![434](images/Pasted%20image%2020250505175446.png)

## 2.7 定位

元素定位分为**无定位**、**绝对定位**、**相对定位**和**固定定位**四种。元素定位是根据参照物来进行定位，定位时根据元素与参照物上下左右四个方向中任意相邻的两个方向的距离来进行定位，定位方式不同，参照物也不一样。元素定位默认为无定位。**绝对定位和固定定位的元素必须设置宽度和高度。**

| 属性值      | 说明                |
| -------- | ----------------- |
| static   | 默认值，没有定位          |
| relative | 相对自身进行定位          |
| absolute | 绝对含有定位的最近的父容器进行定位 |
| fixed    | 相对于浏览器窗口进行固定定位    |

```CSS
position: relative;
top: 10px;
left: 10px;

position: absolute;
top: 10px;
right: 10px;

position:fixed;
left: 20px;
bottom: 20px;
```

### 2.7.1 举例 

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>元素定位</title>  
    <link href="css/position.css" type="text/css" rel="stylesheet">  
</head>  
<body>  
  
    <div class="box">  
        <div class="p1"></div>  
        <div class="p2"></div>  
    </div>  
    <input class="fixed" type="button" value="按钮">  
  
</body>  
</html>
```

```CSS
html, body {  
    width: 100%;  
    height: 100%;  
    margin: 0;  
    padding: 0;  
}  
  
.box {  
    height: 400px;  
    background-color: darkseagreen;  
    margin: 40px;  
}  
  
.p1 {  
    height: 100px;  
    background-color: lightpink;  
    /* 相对定位：参照物即是自身 */    
    position: relative;  
    top: 10px;  
    left: 20px;  
}  
  
.p2 {  
    height: 100px;  
    width: 100%;  
    background-color: #dbb6ff;  
    /* 绝对定位：参照物是body，且必须要设置宽和高 */    
    /* 绝对定位的参照物是距离该元素最近的，含有定位为relative/absolute的父容器，若没有，则参照物为body */  
    position: absolute;  
    top: 0;  
    left: 0;  
}  
  
.fixed {  
    width: 80px;  
    height: 80px;  
    /* 固定定位：相对于浏览器窗口进行固定定位 */    
    position: fixed;  
    top: 0;  
    right: 0;  
}
```

![](images/Pasted%20image%2020250505181719.png)

## 2.8 列表样式

|属性|含义|示例|
|---|---|---|
|list-style-type|设置列表每一项前面的修饰类型|list-style-type:circle;|
|list-style-image|设置列表每一项前面的图片|list-style-image:url(’图片路径’);|
|list-style-position|设定列表每一项前面的修饰位置|list-style-position:inside;|
|list-style|在一个声明中设置所有列表属性|list-style:circle (’图片路径’) inside|
### 2.8.1 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>元素定位</title>  
    <link href="css/position.css" type="text/css" rel="stylesheet">  
</head>  
<body>  

    <ul>        
    	<li>列表项1</li>  
        <li>列表项2</li>  
        <li>列表项3</li>  
    </ul> 
    
</body>  
</html>
```

```CSS
ul {  
    list-style-type: decimal; /* 序号为数字标号1、2、3…… */  
}
```


![](images/Pasted%20image%2020250505182154.png)

```CSS
ul {  
    list-style-type: decimal; /* 序号为数字标号1、2、3…… */  
    list-style-image: url("../img/微草队徽.jpg");  
    list-style-position: inside;  
}
```

![](images/Pasted%20image%2020250505182340.png)

## 2.9 伪类样式

常用的伪类样式是鼠标悬浮的伪类样式：hover。

```CSS
div:hover{
	background: red;
}
```

超链接伪类样式如下。

|伪类名称|含义|示例|
|---|---|---|
|a:link|未单击访问时超链接样式|a:link{color:black;}|
|a:visited|单击访问后超链接样式|a:visited{color:pink;}|
|a:hover|鼠标悬浮其上的超链接样式|a:hover{color:red;}|
|a:active|鼠标单击未释放的超链接样式|a:active{color:orange;}|

当超链接同时拥有上面的伪类样式时，其书写顺序有要求：  
a:link->a:visited->a:hover->a:active  

### 2.9.1 举例

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>元素定位</title>  
    <link href="css/position.css" type="text/css" rel="stylesheet">  
</head>  
<body>
    
    <div class="text">  
        鼠标放置于上时会发生变化  
    </div>   
  
	<a href="">  
	    超链接  
	</a> 
  
</body>  
</html>
```

```CSS
.text:hover {  
    background-color: darkslategrey;  
}

a:link {  
    color: black;  
}  
a:visited {  
    color: darkred;  
}  
a:hover {  
    color: forestgreen;  
}  
a:active {  
    color: cornflowerblue;  
}
```

![](images/Pasted%20image%2020250505182636.png)

![](images/Pasted%20image%2020250505183123.png)

# 3 盒子模型

HTML 中的每一个元素都是一个容器，这个容器就像是一个盒子，它包括：外边距，边框，填充，和实际内容。

![image 25.png](images/image%2025.png)

元素的总宽度 = 左外边距 + 左边框 + 左内边距 + 宽度 + 右内边距 + 右边框 + 右外边距

元素的总高度 = 上外边距 + 上边框 + 上内边距 + 高度 + 下内边距 + 下边框 + 下外边距

# 4 CSS 3 新特性

## 4.1 边框

**border-radius:** 用于创建圆角

```CSS
#border{
	width: 100px;
	height: 100px;
	background-color: red;
	border-radius: 5px;
}
```

## 4.2 盒子阴影

box-shadow: 用来添加阴影

```CSS
box-shadow:阴影类型 水平阴影位置 垂直阴影位置 阴影模糊距离 阴影大小 阴影颜色;
```

```CSS
box-shadow: inset 2px 2px 2px 2px red;
```

## 4.3 CSS 3

### 4.3.1 线性渐变—Linear Gradients

颜色沿着一条直线过渡：从左到右、从右到左、从上到下等

```CSS
linear-gradient(渐变方向,  颜色1,  yanse2, ..., 颜色n)
```

```CSS
.block1 {
	/* 从上到下的线性渐变： */
	background: linear-gradient(red, blue);
}
.block2  {
	/* 从左到右的线性渐变：*/
	background: linear-gradient(to right,red, blue);
}
.block3 {
	/* 从左上角到右下角的线性渐变：*/
	background: linear-gradient(to bottom right, red , blue);
}
```

### 4.3.2 径向渐变—Radial Gradients

圆形或椭圆形渐变，颜色不再沿着一条直线变化，而是从一个起点朝所有方向混合

```CSS
radial-gradient(center, shape size, start-color, ..., last-color);
```

```CSS
.block1 {
	/* 颜色结点均匀分布的径向渐变：*/
	background: radial-gradient(red, green, blue);
}
.block2 {
	/* 颜色结点不均匀分布的径向渐变： */
	background: radial-gradient(red 5%, green 15%, blue 60%);
}
.block3 {
	/* 形状为圆形的径向渐变：*/
	width: 600px;height: 400px;
	background: radial-gradient(circle, red, yellow, green);
}
```

## 4.4 文本效果

### 4.4.1 text-shadow

向文本添加阴影。

文本阴影与盒子阴影的区别：文本阴影无内外之分，且文本阴影没有阴影大小的设置。

|值|说明|
|---|---|
|h-shadow|必需，水平阴影的位置，允许负值|
|v-shadow|必需，垂直阴影的位置，允许负值|
|blur|可选，模糊举例|
|color|可选，阴影的颜色|

### 4.4.2 text-overflow

当文本溢出包含元素时发生的事情，超出部分显示省略号。

- white-space:nowrap 文本不会换行，在同一行继续
- overflow:hidden 溢出隐藏
- text-overflow:ellipsis 用省略号来代表被修剪的文本

```CSS
/*文本阴影与盒子阴影的区别在于：文本阴影无内外之分，且文本阴影没有阴影大小的设置*/
text-shadow: 2px 2px 2px red;
/*文本溢出时不换行*/
white-space: nowrap;
/*元素溢出部分隐藏掉*/
overflow: hidden;
/*文本溢出部分使用省略号显示*/
text-overflow: ellipsis;
```

## 4.5 字体

```CSS
@font-face {
		font-family: 必需。规定字体的名称
		src: 必需。定义字体文件的 URL
		font-weight: 可选。定义字体的粗细。默认是 "normal"
		font-style: 可选。定义字体的样式。默认是 "normal"
}
```

## 4.6 变形

CSS 3 变形是一些效果的集合。如平移、旋转、缩放、倾斜效果；每个效果都可以称为**变形（transform）**，它们可以分别操控元素发生平移、旋转、缩放、倾斜等变化。

```CSS
/* transform-function是一个变形函数，可以是一个，也可以是多个，中间以空格分开 */
transform:[transform-function];
```

### 4.6.1 平移

translate(x, y)：平移函数，基于 X、Y 坐标重新定位元素的位置

translateX(x)：表示只设置 X 轴的位移

translateY(y)：表示只设置 Y 轴的位移

```CSS
/* transform: translate(20px, 30PX); */
/* transform: translateX(20px); */
transform: translateY(20px);
```

### 4.6.2 2 D 缩放

scale(x, y)：缩放函数，可以使任意元素对象尺寸发生变化。当该函数只接收一个值时，表示同时设置 X 与 Y 的值

scaleX(x)：表示只设置 X 轴的缩放

scaleY(y)：表示只设置 Y 轴的缩放

```CSS
/*transform: scale(0.5, 1.5);*/
/*transform: scaleX(0.5);*/
transform: scaleY(0.5);
```

### 4.6.3 旋转

rotate(degree)：旋转函数，取值是一个度数值。参数 degree 单位使用 deg 表示，参数 degree 取正值时元素相对原来中心顺时针旋转

```CSS
transform: rotate(10deg);
```

### 4.6.4 倾斜

skew(x, y)：倾斜函数，取值是一个度数值。

skewX(x)：表示只设置 X 轴的倾斜

skewY(y)：表示只设置 Y 轴的倾斜

```CSS
/*transform: skew(20deg, 60deg);*/
/*transform: skewX(45deg);*/
transform: skewY(45deg);
```

## 4.7 过渡

**transition** 呈现的是一种过渡，是一种动画转换的过程，如渐现、渐弱、动画快慢等。

CSS 3 transition 的过渡功能通过一些 CSS 的简单动作触发样式平滑过渡。

```CSS
transition:[transition-property  transition-duration  transition-timing-function transition-delay ] 
```

### 4.7.1 transition-property

过渡或动态模拟的 CSS 属性，为了方便，一般都指定 all，表示所有属性。

### 4.7.2 transition-duration

完成过渡所需要的时间，即从设置旧属性到换新属性所花费的时间，单位为秒（s）  。

### 4.7.3 transition-timing-function

指定过渡函数。

|函数|说明|
|---|---|
|linear|规定以相同速度开始至结束的过渡效果|
|ease|规定慢速开始，然后变快，然后慢速结束的过渡效果（默认值）|
|ease-in|规定以慢速开始的过渡效果|
|ease-out|规定以慢速结束的过渡效果|
|ease-in-out|规定以慢速开始和结束的过渡效果|

### 4.7.4 transition-delay

过渡开始出现的延迟时间。

|值|说明|
|---|---|
|正值|表示元素过渡效果不会立即触发，当过了设置的时间值后才会被触发|
|负值|表示元素过渡效果会从该时间点开始显示，之前的动作被截断；|
|0|默认值，元素过渡效果立即执行|

### 4.7.5 过渡效果的触发时机

- 伪类触发：:hover :active :focus :checked
- 媒体查询：通过@media 属性判断设备的尺寸，方向等
- JavaScript 触发：用 JavaScript 脚本触发

```CSS
#tran{
	width: 200px;
	height: 200px;
	background-color: red;
	/*宽度发生变化时就会触发过渡效果*/
	transition: width .5s ease 0s;
}
#tran:hover{
	width: 50px;
}
```

## 4.8 媒体查询

```CSS
@media mediatype and|not|only (media feature) {
	CSS-Code;
}
```

- mediatype : 表示媒体类型
    - all：用于所有设备
    - screen：用于电脑屏幕，平板电脑，智能手机等。
- media feature ：表示媒体功能
    - max-width：定义输出设备中的页面最大可见区域宽度。
    - min-width：定义输出设备中的页面最小可见区域宽度。

```CSS
.box{
	background-color: red;
	height: 50px;
}

@media screen and (min-width: 700px){
	.box{
		width: 200px;
	}
}

@media screen and (min-width: 900px){
	.box{
		width: 300px;
	}
}

@media screen and (min-width: 1200px){
	.box{
		width: 400px;
	}
}
```