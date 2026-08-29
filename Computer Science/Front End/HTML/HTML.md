---
tags:
  - 前端
  - HTML
---

# 1.简介

## 1.1 概念

HTML 是 Hyper Text Markup Language 的简称，即**超文本标记语言**，是一种用于创建网页的标准标记语言。

HTML 运行在浏览器上，由浏览器来解析执行。

## 1.2 HTML 语言的由来和发展

HTML 的全称是 Hypertext Markup Language （超文本标记语言）用于描述网页文档的标记语言。

### 1.2.1 由来

互联网始于 1969 年的美国，又称因特网。

是美军在[ARPA]（[阿帕网]，[美国国防部研究计划署]制定的协定下，首先用于军事连接，后将美国西南部的[加利福尼亚大学]分校、[斯坦福大学]研究学院、UCSB（[加利福尼亚大学]）和[犹他州大学] 的四台主要的计算机连接起来。

这个协定由[剑桥大学]的 BBN 和 MA 执行，在 1969 年 12 月开始联机。

1989 年，在普及互联网应用的历史上又一个重大的事件发生了。蒂姆-伯纳斯·李和其他在欧洲粒子物理实验室的人这些人在[欧洲粒子物理研究所]非常出名，提出了一个分类互联网信息的协议。这个协议，1991 年后称为[wwb]，基于[超文本协议]就是在一个文字中嵌入另一段文字的链接的系统，当你阅读这些页面的时候，你可以随时用他们选择一段文字[链接]。

### 1.2.2 发展

经过二十多年的发展，HTML 已经成为 IT 时代最重要的技术！HTML 标准经历了如下版本更换：

- HTML 1.0：在 1993 年 6 月作为互联网工程工作小组(IETF)工作草案发布，由此超文本标记语言第一版诞生。
- HTML 2.0：1995 年 11 月作为 RFC 1866 发布，于 2000 年 6 月发布之后被宣布已经过时。
- HTML 3.2：1997 年 1 月 14 日，`W3C` 推荐标准。
- HTML 4.0：1997 年 12 月 18 日，`W3C` 推荐标准。
- HTML 4.01（微小改进）：1999 年 12 月 24 日，`W3C` 推荐标准。
- HTML 5：HTML 5 是公认的下一代 Web 语言，极大地提升了 Web 在富媒体、富内容和富应用等方面的能力，被喻为终将改变移动互联网的重要推手。 2014 年 10 月 28 日，`W3C` 推荐标准。

## 1.3 前端通信原理

### 1.3.1 通信闭环-发送请求

1. 我们在浏览器地址栏输入网址，按下回车发起'请求'
2. 浏览器应用帮我们封装数据打包发送请求
3. 请求发送至本地路由器
4. 路由器将请求送至交换机
5. 交换机将请求送至 DNS 服务器，解析请求地址。将请求通过层层网络服务器(城市之间的电缆，海底的光纤等)

发送至目标网址

ps：发送到目的地其实也需要通过 DNS 服务器先将请求发送到目标地址所在的交换机，然后目标地址所在的交换机转发到目标服务器所在的路由器，最后由目标服务器所在路由器发送请求至目标服务器。

### 1.3.2 通信闭环-接收响应

6. 目标服务器接收到请求，根据请求做出对应'响应'。由于请求中包含有发送方地址信息，因此响应时自动拥有一个目标。
7. 将响应发送至服务器所在路由器
8. 响应经由服务器所在路由器转发至服务器所在交换机
9. 服务器所在交换机将响应送至 DNS 服务器，解析响应地址。将响应通过层层网络服务器发送至请求发送方
10. 同理可知，此时发送方想要接收响应，必然通过交换机->路由器->浏览器这一步骤
11. 最后浏览器识别发来的数据，并加载到页面中，呈现于我们面前。

## 1.4 DNS 与域名及 `ip` 地址

### 1.4.1 DNS

域名系统 Domain Name System，为网络上机器命名的一种解决办法。

DNS 规定：网络中每一台机器必然拥有一个名字。

DNS 规定：网络中一台机器想要访问另一台机器，必须先知道另一台机器的名字。

### 1.4.2 `ip` 地址：

上面提到的名字就是 `ip` 地址。`ip` 地址是由四段以“.”分开的数字组成（早期）

### 1.4.3 域名

因为一串数字并不那么好记(不信你记一下 202.108.22.5)，因此域名出现了。可以认为域名是网络上一台机器的昵。

### 1.4.4 DNS 服务器

网络上的机器都有 `ip` 地址，在网络上通信需要 `ip` 地址。然而我们记不住 `ip` 地址，我们能记住的只有域名

为了能我们写域名，机器就知道对应的 `ip` 地址，DNS 服务器出现了。

DNS 服务器 Domain Name Server 是一种程序，它保存了一张域名(domain name)和与之相对应的 `IP` 地址 (IP address)用来帮助我们解析消息的域名。

## 1.5 基本结构

```HTML
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>01大学</title>
	</head>
	<body></body>
</html>
```

### 基本结构说明

```HTML
<!DOCTYPE html> 表示定义的文档类型为 HTML5 文档。

<html></html> 表示整个 HTML 文档内容的定义只能在该标签对之间

<head></head> 表示整个 HTML 文档的头部信息，比如文档的标题、文档使用的字符集编码、文档是否可以缩放等。

<meta charset="utf-8"> 表示定义文档的字符集编码为 "utf-8"，支持中文

<title></title> 表示定义文档显示的标题

<body></body> 表示 HTML 文档的主体内容部分应该定义在该标签内
```

p.s. 标签一般都是成对出现，分别叫==开放标签==和==闭合标签==

# 2. HTML 标签

## 2.1 分类

HTML 标记也被称为是标签，浏览器厂商将其称之为元素

HTML 标签分为两大类：
1. 块级标签 / 行级元素 / 块元素（block elements）
2. 行级标签 / 行内元素 / 行元素（inline-block elements）

- 区别：块级元素独占一行，行内元素不独占一行

## 2.2 块级标签 / 行级元素 / 块元素

### 2.2.1 块级标签特征

1. 总是在新行上开始
2. 高度，行高以及外边距和内边距都可控制
3. 宽度，是它的容器的 100%
4. 可以容纳内联元素和其他块元素

### 2.2.2 标题标签

表示一段文字的标题或主题，并且支持多层次的内容结构。

```HTML
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
```

![[image 24.png|image 24.png]]

### 2.2.3 水平线标签

表示一条水平线，该元素比较特殊，没有结束元素，闭合标签。

```HTML
<hr>
```

![[image 1 9.png|image 1 9.png]]

### 2.2.4 段落标签

标题标签表示一段文字的标题或主题，并且支持多层次的内容结构。

```HTML
<p>
	<!-- 段落内容  -->
</p>
```

![[image 2 8.png|image 2 8.png]]

### 2.2.5 无序列表标签

由 `<ul>` 元素和 `<li>` 元素组成，使用 `<ul>` 元素作为无序列表的声明，使用 `<li>` 元素作为每个列表项的起始。遵循 `W3C` 标准，`<ul>` 元素里面只能嵌套 `<li>` 元素，不能嵌套其他元素。 `<*li>` 元素里面可以嵌套任意元素。

特性：

1. 没有顺序，每个 `<*li>` 元素独占一行。
2. 默认 `<li>` 元素项前面有个实心小圆点。
3. 一般用于无序类型的列表，如导航、边栏新闻、有规律的图文组合模块等。

```HTML
<ul>
	<li>列表项1</li>
	<li>列表项2</li>
	<li>列表项3</li>
	……
	<li>列表项n</li>
</ul>
```

![[image 3 5.png|image 3 5.png]]

### 2.2.6 有序列表标签

由 `<ol>` 元素和 `<li>` 元素组成，使用 `<ol>` 元素作为有序列表的声明，使用 `<li>` 元素作为每个列表项的起始，有序列表嵌套同无序列表一样，只能在 `<ol>` 元素里嵌套 `<li>` 元素。

特性：

1. 有顺序，每个 `<li>` 元素独占一行。
2. 默认 `<li>` 元素项前面有顺序标记。
3. 一般用于排序类型的列表，如试卷、问卷选项等。

```HTML
<ol>
	<li>列表项1</li>
	<li>列表项2</li>
	<li>列表项3</li>
	……
	<li>列表项n</li>
</ol>
```

![[image 4 4.png|image 4 4.png]]

```HTML
<h1>热搜</h1>  
<hr>  
<!-- type 表示序号的类型，可自选，默认是1 -->
<ol type="A">  
    <li>八一八青少校和温少尉的社会主义兄弟情</li>  
    <li>点我就看青温在线吵架</li>  
    <li>永远保护你</li>  
</ol>
```

![[Pasted image 20250502220342.png]]

### 2.2.7 定义列表

是一种很特殊的列表形式，它是标题及列表项的结合。定义列表的语法相对于无序和有序列表不太一样，它使用 `<dl>` 元素作为列表的开始，使用 `<dt>` 元素作为每个列表项的起始而对于每个列表项的定义则使用 `<dd>` 元素来完成。

```html
<dl>
    <dt>丹宁色（半光饰面）</dt>
    <dd>天花板</dd>

    <dt>丹宁色（蛋壳光饰面）</dt>
    <dt>暮色天空（蛋壳光饰面）</dt>
    <dd>分层涂刷于墙面</dd>
</dl>
```

特性：

1. 没有顺序，每个 `<dt>` 元素，`<dd>` 元素独占一行。
2. 默认没有标记。
3. 一般用于（一个标题下有一个或多个列表项）* n 的情况。

#### 2.2.7.1 列表总结

1. 无序列表中的每项都是平级的，没有级别之分，并且列表中的内容一般都是相对简单的标题性质的网页内容。有序列表会依据列表项的顺序进行显示。
2. 在实际的网页应用中，无序列表比有序列表应用得更加广泛。有序列表 ol-li 一般用于显示带有顺序编号的特定场合。
3. 定义列表一般适用于带有标题和标题解释性内容的场合。

### 2.2.8 表格标签

```HTML
<table border="1"><!--border表示单元格边框大小-->

  <caption>表格的标题</caption><!--表格的标题-->

  <thead><!--表格的头部-->
    <tr><!--表格头部中的行-->
      <th>列名1</th><!--表格头部中的列-->
      <th>列名2</th>
      ......
      <th>列名n</th>
    </tr>
  </thead>

  <tbody><!--表格的主体部分-->
    <tr><!--表格主体部分中的行-->
      <td>列1的值</td><!--表格主体部分中的列-->
      <td>列2的值</td>
      ......
      <td>列n的值</td>
    </tr>
    <tr>
      <td>列1的值</td>
      <td>列2的值</td>
      ......
      <td>列n的值</td>
    </tr>
    ......
    <tr>
      <td>列1的值</td>
      <td>列2的值</td>
      ......
      <td>列n的值</td>
    </tr>
  </tbody>

  <tfoot><!--表格的尾部-->
    <tr><!--表格尾部中的行，主要用于信息统计-->
      <td>统计项名称</td><!--表格尾部中的列-->
      <td>列1的值</td>
      ......
      <td>列n的值</td>
    </tr>
  </tfoot>

</table>
```

![[image 5 4.png|image 5 4.png]]

#### 2.2.8.1 不规则表格

```HTML
<table border="2">  
    <caption><h3>青温tag日增表</h3></caption>  
  
    <thead>    
    <tr>        
    	<!-- rowspan 表示行的范围 -->  
        <!-- colspan 表示列的范围 -->  
        <td rowspan="2" colspan="1">类型</td>  
        <td rowspan="1" colspan="5">tag数</td>  
    </tr>    
    <tr>        
    	<td>1月</td>  
        <td>2月</td>  
        <td>3月</td>  
        <td>4月</td>  
        <td>5月</td>  
    </tr>    
    </thead>  
    
    <tbody>    
    <tr>        
    	<td>原著向</td>  
        <td>12</td>  
        <td>42</td>  
        <td>31</td>  
        <td>27</td>  
        <td>16</td>  
    </tr>  
    <tr>        
    	<td>哨向paro</td>  
        <td>2</td>  
        <td>4</td>  
        <td>10</td>  
        <td>7</td>  
        <td>6</td>  
    </tr>  
    </tbody>  
    
</table>
```

![[Pasted image 20250502222432.png]]

### 2.2.9 层标签

```HTML
<div>
	<!--内容-->
</div>
```

![[image 6 3.png|image 6 3.png]]

### 2.2.10 表单

主要用于采集数据，然后发送数据。

```HTML
<form action="请求资源" method="请求方式">
	......
</form>
```

```HTML
<form action="test2.html" method="get">
    <input type="submit" value="转跳到test2界面">
</form>
```

![[image 7 2.png|image 7 2.png]]

## 2.3 行级标签

### 2.3.1 行级标签特征

1. 和其他元素都在一行上；
2. 高度，行高及外边距和内边距不可改变；
3. 宽度就是其内容的宽度，不可改变；

### 2.3.2 图像标签

```html
<img src="图片地址" alt="图片的替代文字（破图显示）" title="鼠标悬停提示文字" width="图片宽度" height="图片高度" />
```

```HTML
<img src="logo.png" title="鼠标放在上面显示的内容" alt="图片未加载时显示">
```

![[image 8 2.png|image 8 2.png]]

### 2.3.3 范围标签

```HTML
<span>内容</span>
```

![[image 9 2.png|image 9 2.png]]

### 2.3.4 超链接标签

```HTML
<a href="资源地址" target="目标窗口位置">内容</a>
```

常用 target：

```HTML
_blank <!--在新窗口中打开-->

_self <!--在当前窗口中打开，是超链接target属性的默认值-->
```

![[image 10 2.png|image 10 2.png]]

超链接通常分为**页面间链接**、**锚链接**和**功能性链接**。

- 页面间链接
    
    ```HTML
    <a href="页面名称">内容</a>
    ```
    
    ```HTML
    <a href="http://www.baidu.com" target="_self">前往百度</a>
    ```
    
    ![[image 11 2.png|image 11 2.png]]
    
- 锚链接
    
    ```HTML
     <a href="页面名称\#元素的ID属性值">内容</a>
    ```
    
    锚链接定位同一个页面中的元素时，页面名称可以省略。
    
    ```HTML
    <a id="top" href="#bottom">去底部</a>
    
    <p id="bottom">  
	    底部  
	</p>
    ```
    
    ![[image 12 2.png|image 12 2.png]]
    
    ![[image 13 2.png|image 13 2.png]]
    
- 功能性链接
    
    ![[image 14 2.png|image 14 2.png]]
    

### 2.3.5 输入标签

```HTML
<input type="类型" name="名称" value="值">
```

| 属性        | 说明                                                                                                              |
| --------- | --------------------------------------------------------------------------------------------------------------- |
| type      | 指定元素的类型。text、password、checkbox、radio、submit、reset、fille、hidden、image、button、number、date、datetime-local，默认为 text。 |
| name      | 指定表单元素的名称。除了有采集数据的功能外，还具有分组的功能，同一组内的单选按钮 radio 只能选择一个。                                                          |
| value     | 元素的初始值。type 为 radio 时必须指定一个值。                                                                                      |
| size      | 指定表单元素的初始宽度。当 type 为 text 或 password 时，表单元素的大小以字符为单位。对于其他类型，宽度以像素为单位。                                                 |
| maxlength | type 为 text 或 password 时，输入的最大字符数。                                                                                   |
| checked   | type 为 radio 或 checkbox 时，指定按钮是否被选中。                                                                                 |

![[image 15 2.png|image 15 2.png]]

```HTML
<form>  
    <input type="text" name="username">  
    <input type="password" name="password">  
    <!-- name 属性可分组 -->  
    <!-- checked 设置为默认值-->  
    <!-- 需要注意的是，若input标签type值为radio或checkbox时，只要标签上存在checked属性，那么该标签就会出错-->  
    <input type="radio" value="F" name="sex" checked>女  
    <input type="radio" value="M" name="sex">男  
    <input type="radio" value="O" name="sex">其他  
    <!-- 复选框 -->  
    <input type="checkbox" value="1" name="channel" checked>报纸  
    <input type="checkbox" value="2" name="channel">网络  
    <input type="checkbox" value="3" name="channel">其他  
    <input type="file" name="head">  
    <!-- 当input的type值为submit时，按钮可以提交表单，提交的表单通过name属性采集数据-->  
    <input type="submit" value="按钮">  
</form>
```

```HTML
<input type="hidden" name="名称"> <!-- 隐藏域 -->
```

### 2.3.6 文本域

```HTML
<textarea name="名称" placeholder="提示信息"></textarea>
```

![[image 16 2.png|image 16 2.png]]

```HTML
<!-- label标签和span标签都具有范围选择的功能 -->  
<label>贴膜</label>  
<label>  
    <textarea name="贴膜" placeholder="请输入你最喜欢的青温片段"></textarea>  
</label>  
  
<form action="" method="get">  
    <div>        
    	<!-- for属性一定是一个input输入框的id值，表示label标签被点击时，对应的input框获得焦点 -->  
        <label for="input1">账号：</label>  
        <input id="input1" type="text" name="username">  
    </div>
    </form>
```

### 2.3.7 下拉列表框

```HTML
<select>
		<option value="值">显示值</option>
		<option value="值">显示值</option>
		......
		<option value="值">显示值</option>
</select>
```

![[image 17 2.png|image 17 2.png]]

### 2.3.8 只读和禁用

```HTML
<input type="类型" name="名称" readonly>  <!-- 只能读，不能修改 -->
<input type="类型" name="名称" disabled>  <!-- 禁用 -->
<select name="名称" disabled> <!-- 禁用 -->
		<option value="值">显示值</option>
		<option value="值">显示值</option>
		......
		<option value="值">显示值</option>
</select>
<textarea name="名称" placeholder="提示信息" readonly></textarea><!-- 只能读，不能修改 -->
<textarea name="名称" placeholder="提示信息" disabled></textarea><!-- 禁用 -->
```

![[image 18 2.png|image 18 2.png]]

### 2.3.9 字体加粗

它是一个带有语义化的元素，它有强调、加强语气的作用。

```html
<div>普通文本</div>
<strong>加粗文本</strong>
```

![[Pasted image 20260613224149.png]]

### 2.3.10 字体倾斜

```html
<div>普通文本</div>
<em>倾斜文本</em>
```

![[Pasted image 20260614173959.png]]

### 2.3.11 换行标签

`<br/>` 表示直接下一行。没有结束元素，闭合标签。

```html
<div>换行</div>
<br/>
<div>换行</div>
```

![[Pasted image 20260614174458.png]]

# 3. 综合应用

## 3.1 登录表单

![[Pasted image 20250503105936.png]]

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Log in</title>  
</head>  
<body>  
  
    <form action="" method="get">  
        <div>            
        	<label>账号：</label>  
            <input type="text" name="username">  
        </div>        
        <div>            
        	<label>密码：</label>  
            <input type="password" name="password">  
        </div>        
        <div>            
        	<input type="submit" value="登录">  
        </div>    
    </form>
    
</body>  
</html>
```

## 3.2 注册表单

![[Pasted image 20250503111236.png]]

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Register</title>  
</head>  
<body>  
  
    <!-- &nbsp; 表示的是空格的转义字符 -->  
  
    <form action="" method="get">  
        <div>            
        	<label>账&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;号</label>  
            <input type="text" name="username">  
        </div>        
        <div>            
        	<label>密&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;码</label>  
            <input type="password" name="password">  
        </div>        
        <div>            
        	<label>确认密码</label>  
            <input type="password" name="confirmPassword">  
        </div>        
        <div>            
        	<label>性&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;别</label>  
            <input type="radio" value="M" name="sex">男  
            <input type="radio" value="F" name="sex">女  
            <input type="radio" value="O" name="sex">其他  
        </div>  
        <div>            
        	<label>国&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;籍</label>  
            <select>                
            	<option value="">请选择</option>  
                <option value="CN">中国</option>  
                <option value="FR">法国</option>  
            </select>        
        </div>        
        <div>            
        	<input type="submit" value="注册">  
        </div>    
    </form>  
    
</body>  
</html>
```

## 3.3 查询页面

![[Pasted image 20250503112445.png]]

```HTML
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>SELECT</title>  
</head>  
<body>  
    <div>        
    	<label>姓名</label>  
        <input type="text" name="name">  
        <label>性别</label>  
        <select>            
        	<option value="">请选择</option>  
            <option value="M">男</option>  
            <option value="F">女</option>  
            <option value="O">其他</option>  
        </select>        
        <input type="button" value="查询">  
    </div>  
    <table border="1" width="100%">  
  
        <thead>        
        <tr>            
        	<th>姓名</th>  
            <th>性别</th>  
            <th>年龄</th>  
            <th>成绩</th>  
        </tr>        
        </thead>  
        
        <tbody>        
        <tr>            
        	<th>李四</th>  
            <th>男</th>  
            <th>22</th>  
            <th>76</th>  
        </tr>        
        <tr>            
        	<th>龙华</th>  
            <th>女</th>  
            <th>23</th>  
            <th>85</th>  
        </tr>        
        <tr>            
        	<th>金凤</th>  
            <th>女</th>  
            <th>22</th>  
            <th>89</th>  
        </tr>        
        </tbody>  
        
    </table>  
</body>  
</html>
```

# 4. HTML 5 新增元素

## 4.1 结构标签

|标签|说明|
|---|---|
|header|页面页眉|
|nav|页面导航|
|main|页面主要内容区块|
|section|页面内容区块|
|article|独立的内容块|
|aside|侧边栏|
|footer|页面脚注|

### 举例

```HTML
<!DOCTYPE html>
<html>
<head>
		<meta charset="UTF-8">
		<title>超用心在线教育</title>
		<style>
				html,body{
						width: 100%;
						height: 100%;
						margin: 0;
						padding: 0;
				}
				header,footer {
						height: 40px;
						background-color: black;
						color: white;
				}
				main{
						height: calc(100% - 40px);
						display: grid; /* 网格布局 */
						grid-template-columns: 200px calc(100% - 200px);
				}
				aside{
						background-color: brown;
				}
				section{
						background-color: red;
						display: grid;
						grid-template-rows: 40px calc(100% - 80px) 40px;
				}
				nav{
						background-color: rebeccapurple;
				}
		</style>
</head>
<body>
		<header>页面页眉</header>
		<!-- 页面主体部分 -->
		<main>
				<aside>侧边栏</aside>
				<!-- 页面内容区块 -->
				<section>
						<nav >操作导航</nav>
						<article>独立内容块</article>
						<footer>页面脚注</footer>
				</section>
		</main>
</body>
</html>
```

![[image 19 2.png|image 19 2.png]]

## 4.2 其他标签

|标签|说明|
|---|---|
|audio|定义音频，如音乐或其他音频流|
|video|定义视频，如电影片段或其他视频流|
|canvas|定义图形|
|datalist|定义可选数据的列表|
|time|标签定义日期或时间|
|mark|在视觉上向用户呈现那些需要突出的文字|

### 音频标签

```HTML
<!-- controls属性控制页面上是否显示音频的操作控件
		autoplay属性表示音频在就绪后马上播放
		loop属性表示音频结束后重新播放
		preload值：
		auto - 当页面加载后载入整个音频
		metadata - 当页面加载后只载入元数据
		none - 当页面加载后不载入音频-->
<audio src="音频路径" controls="controls" autoplay="autoplay" loop="loop" preload="metadata"></audio>
```

常见的音频格式：MP 3、OGG、Wav

音频标签还支持设置多个音频文件。

```HTML
<audio controls="controls" autoplay="autoplay" loop="loop" preload="metadata">
		<source src="音频路径1"/>
		<source src="音频路径2"/>
		浏览器不支持该音频格式
</audio>
```

### 视频标签

```HTML
<video src="视频路径" controls="controls" autoplay="autoplay" loop="loop" preload="metadata">
</video>
```

常见的视频格式：avi、flv、mp 4、mkv、ogv

视频标签的用法与 audio 标签一样。

### 列表标签

```HTML
<input list="id">
<datalist id="id">
		<option>选项1</option>
		<option>选项2</option>
		<option>选项3</option>
</datalist>
```

### 时间与标记标签

```HTML
<p>
		 <!--时间标签没有什么实际意义，只是供机器识别：比如搜索引擎、爬虫分析-->
		我在<time datetime="2021-02-14">情人节</time>有个约会
</p>
```

```HTML
<p>
		她长得很<mark>漂亮</mark>
</p>
```

# 5. HTML 5 新增元素属性

## 5.1 全局属性

|属性|说明|
|---|---|
|contentEditable|元素是否允许可编辑内容|
|spellcheck|是否必须对元素进行拼写或语法检查|
|tabindex|指定元素的 tab 键选择次序|

```HTML
<div style="height: 100px" hidden></div>
<div style="height: 100px" contenteditable="true" spellcheck="true" 
tabindex="3"></div>
<div style="height: 100px" contenteditable="true" spellcheck="true" 
tabindex="2"></div>
<div style="height: 100px" contenteditable="true" spellcheck="true" 
tabindex="1"></div>
```

## 5.2 表单属性

|属性|说明|
|---|---|
|placeholder|指定元素的默认提示信息|
|required|元素内容为必填|
|pattern|使用正则表达式检测元素内容是否合法|

```HTML
<form action="" method="get">
		<input type="text" placeholder="请输入账号" required pattern="[a-z]{8,15}" title="账号只能为8到15位">
		<input type="submit" value="注册">
</form>
```