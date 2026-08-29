---
tags:
  - 前端
---
# 1.HTML&CSS

## 1.1 HTML 5 新特性

[[HTML]]

## 1.2 CSS 3 新特性

[[CSS]]

## 1.3 常见布局

## 1.4 响应式布局

### 1. 定义

**响应式布局（Responsive Web Design, RWD）** 是指网页能够**自动适应不同设备的屏幕尺寸**（如手机、平板、电脑），提供最佳浏览体验。  
核心目标是：**一次开发，多端适配**，无需为每个设备单独开发不同版本。

---

### **2. 三大核心技术**

#### **(1) 弹性网格布局（Flexible Grid）**

- 使用 **相对单位（如 `%`、`fr`、`rem`）** 替代固定像素（`px`）。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}
```

#### **(2) 弹性图片/媒体（Flexible Media）**

- 确保图片和视频能随容器缩放

```css
img, video {
  max-width: 100%;
  height: auto;
}
```

#### **(3) 媒体查询（Media Queries）**

- 根据屏幕宽度应用不同的 CSS 规则：

```css
/* 手机端 */
@media (max-width: 600px) {
  body { font-size: 14px; }
}
/* 平板端 */
@media (min-width: 601px) and (max-width: 1024px) {
  body { font-size: 16px; }
}
/* 桌面端 */
@media (min-width: 1025px) {
  body { font-size: 18px; }
}
```

---

### **3. 实现响应式布局的常见方法**

#### **(1) 视口设置（Viewport Meta Tag）**

- **必须**在 HTML 的 `<head>` 中添加：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

- `width=device-width`：让页面宽度等于设备宽度。
- `initial-scale=1.0`：禁止默认缩放。

#### **(2) 流动布局（Fluid Layout）**

- 使用百分比（`%`）或视口单位（`vw`/`vh`）

```css
.sidebar {
  width: 30%; /* 占父容器的30% */
}
.main {
  width: 70%;
}
```

### **(3) Flexbox 布局**

- 弹性盒子模型，适合一维布局（行或列）

```css
.container {
  display: flex;
  flex-wrap: wrap; /* 允许换行 */
}
.item {
  flex: 1 1 200px; /* 弹性伸缩，最小宽度200px */
}
```

### **(4) CSS Grid 布局**

- 二维网格系统，适合复杂布局：

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}
```

### **(5) 断点（Breakpoints）设计**

- 根据设备宽度设置布局变化点（常用断点）

|设备类型|断点范围|
|---|---|
|手机（竖屏）|`max-width: 600px`|
|平板（竖屏）|`601px - 1024px`|
|桌面端|`min-width: 1025px`|

---

### **4. 响应式布局的实战示例**

#### **HTML 结构**

```html
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>响应式布局示例</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>Header</header>
  <main>
    <section class="content">主要内容</section>
    <aside class="sidebar">侧边栏</aside>
  </main>
  <footer>Footer</footer>
</body>
</html>
```

#### **CSS 实现**

```css
/* 基础样式 */
body {
  margin: 0;
  font-family: Arial;
}

header, footer {
  background: #333;
  color: white;
  padding: 1rem;
  text-align: center;
}

main {
  display: flex;
  flex-wrap: wrap;
}

.content {
  flex: 1 1 70%;
  padding: 1rem;
  background: #f4f4f4;
}

.sidebar {
  flex: 1 1 30%;
  padding: 1rem;
  background: #ddd;
}

/* 手机端：堆叠布局 */
@media (max-width: 600px) {
  main {
    flex-direction: column;
  }
  .content, .sidebar {
    flex: 1 1 100%;
  }
}
```

---

### **5. 响应式布局 vs 自适应布局（Adaptive Layout）**

|特性|响应式布局（RWD）|自适应布局（AWD）|
|---|---|---|
|**原理**|同一套代码，通过 CSS 动态调整|为不同设备提供不同 HTML/CSS|
|**实现方式**|媒体查询 + 弹性布局|服务器端检测设备后返回不同页面|
|**灵活性**|更灵活，适应所有屏幕|需预设设备类型|
|**维护成本**|较低（一套代码）|较高（多套代码）|
|**适用场景**|绝大多数现代网站|旧版浏览器兼容|

---

### **6. 响应式布局的最佳实践**

1. **移动优先（Mobile First）**：先写手机端样式，再用 `min-width` 扩展到大屏。
2. **测试多设备**：使用 Chrome DevTools 或真实设备测试。
3. **优化图片**：使用 `srcset` 提供不同分辨率的图片。
```html
<img src="small.jpg" srcset="medium.jpg 1000w, large.jpg 2000w" alt="示例">
```

4. **避免固定宽度**：多用 `max-width` 而非 `width`。
5. **使用 CSS 变量**：方便统一调整断点和样式。

---

### **7. 总结**

- **响应式布局 = 弹性网格 + 弹性媒体 + 媒体查询**。
- **核心目标**：让网页在任何设备上都能良好显示。
- **推荐工具**：Flexbox + CSS Grid + 媒体查询。
- **移动优先**：从小屏幕开始设计，逐步增强。

## 1.5 Flex布局

Flex 布局（Flexible Box Layout）是 CSS3 提供的一种现代布局模式，用于更高效地**在单行或单列中分配容器内项目的空间**，即使项目的大小未知或动态变化。

---

### **1. Flex 布局的核心概念**

#### **(1) Flex 容器（Flex Container）**

- 通过 `display: flex` 或 `display: inline-flex` 将一个元素定义为 Flex 容器。
- 容器内的直接子元素自动成为 **Flex 项目（Flex Items）**。

#### **(2) 主轴（Main Axis）和交叉轴（Cross Axis）**

- **主轴**：Flex 项目的默认排列方向（可通过 `flex-direction` 修改）。
- **交叉轴**：与主轴垂直的方向。

---

### **2. Flex 容器的属性**

#### **(1) 主轴方向：`flex-direction`**

定义 Flex 项目的排列方向。

```css
.container {
  flex-direction: row;         /* 默认：从左到右（水平） */
  flex-direction: row-reverse; /* 从右到左 */
  flex-direction: column;      /* 从上到下（垂直） */
  flex-direction: column-reverse; /* 从下到上 */
}
```

#### **(2) 换行：`flex-wrap`**

控制项目是否换行：

```css
.container {
  flex-wrap: nowrap; /* 默认：不换行（可能溢出） */
  flex-wrap: wrap;    /* 换行 */
  flex-wrap: wrap-reverse; /* 反向换行 */
}
```

#### **(3) 主轴对齐：`justify-content`**

控制项目在主轴上的对齐方式：

```css
.container {
  justify-content: flex-start;  /* 默认：左对齐 */
  justify-content: flex-end;    /* 右对齐 */
  justify-content: center;     /* 居中 */
  justify-content: space-between; /* 两端对齐，项目间间隔相等 */
  justify-content: space-around;  /* 项目两侧间隔相等 */
  justify-content: space-evenly;  /* 所有间隔完全相等 */
}
```

#### **(4) 交叉轴对齐：`align-items`**

控制项目在交叉轴上的对齐方式：

```css
.container {
  align-items: stretch;    /* 默认：拉伸填满容器高度 */
  align-items: flex-start; /* 顶部对齐 */
  align-items: flex-end;   /* 底部对齐 */
  align-items: center;     /* 垂直居中 */
  align-items: baseline;   /* 基线对齐（按文本对齐） */
}
```

#### **(5) 多行对齐：`align-content`**

控制多行项目在交叉轴上的对齐方式（需 `flex-wrap: wrap`）：

```css
.container {
  align-content: stretch;     /* 默认：拉伸填满 */
  align-content: flex-start;  /* 多行顶部对齐 */
  align-content: center;      /* 多行垂直居中 */
}
```

---

### **3. Flex 项目的属性**

#### **(1) 项目顺序：`order`**

控制项目的排列顺序（数值越小越靠前）：

```css
.item {
  order: 1; /* 默认值为 0 */
}
```

#### **(2) 放大比例：`flex-grow`**

定义项目在剩余空间中的放大比例：

```css
.item {
  flex-grow: 1; /* 默认 0（不放大） */
}
```

- 如果所有项目的 `flex-grow` 为 1，则等分剩余空间。
- 如果某个项目为 2，其他为 1，则它占 2 份，其他占 1 份。

#### **(3) 缩小比例：`flex-shrink`**

定义项目在空间不足时的缩小比例：

```css
.item {
  flex-shrink: 1; /* 默认 1（允许缩小） */
}
```

- 设为 `0` 可禁止项目缩小（避免内容挤压）。

#### **(4) 基准大小：`flex-basis`**

定义项目在分配空间前的初始大小：

```css
.item {
  flex-basis: 200px; /* 可以是 px、%、auto 等 */
}
```

#### **(5) 简写属性：`flex`**

`flex-grow`、`flex-shrink`、`flex-basis` 的简写：

```css
.item {
  flex: 1 0 200px; /* flex-grow | flex-shrink | flex-basis */
}
```

常用缩写：

- `flex: 1` → `flex: 1 1 0`（等分空间）
- `flex: auto` → `flex: 1 1 auto`（基于内容大小分配）

#### **(6) 单独对齐：`align-self`**

覆盖容器的 `align-items` 设置：

```css
.item {
  align-self: center; /* 可取值与 align-items 相同 */
}
```

---

### **4. Flex 布局的常见应用场景**

#### **(1) 水平居中**

```css
.container {
  display: flex;
  justify-content: center;
}
```

#### **(2) 垂直居中**

```css
.container {
  display: flex;
  align-items: center;
}
```

#### **(3) 等高分栏**

```css
.container {
  display: flex;
}
.item {
  flex: 1; /* 等分宽度 */
}
```

#### **(4) 导航栏**

```css
.nav {
  display: flex;
  justify-content: space-between;
}
```

#### **(5) 圣杯布局（Holy Grail Layout）**

```css
.container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.content {
  flex: 1; /* 撑满剩余空间 */
}
```

---

### **5. Flex vs Grid**

|特性|Flex 布局|Grid 布局|
|---|---|---|
|**维度**|一维（行或列）|二维（行和列）|
|**适用场景**|线性排列、内容动态|复杂网格布局|
|**控制粒度**|项目级控制|容器级控制|
|**浏览器支持**|广泛支持（包括 IE10+）|较新浏览器（IE11 部分）|

---

### **6. 总结**

- **Flex 布局 = 弹性容器 + 弹性项目**。
- **核心思想**：通过主轴和交叉轴控制项目的排列、对齐和分布。
- **优势**：
    - 轻松实现居中、等分、动态调整。
    - 比浮动（float）和定位（position）更直观。
- **适用场景**：导航栏、卡片布局、表单排列等单维布局。

掌握 Flex 布局后，你可以告别 `float: left/right` 和 `clearfix` 的繁琐写法，让布局更简洁高效！

## 1.6 Grid 布局

## 1.7 **CSS 单位 `fr` 和 `rem` 详解**

### **1. `fr` 单位（Fractional Unit）**

#### **基本概念**

- **`fr`** 是 CSS Grid 布局中专用的单位，表示**剩余空间的分配比例**。
- 它代表 "fraction"（分数），用于定义网格轨道（行或列）的弹性尺寸。

#### **核心特性**

|特性|说明|
|---|---|
|**仅用于 Grid 布局**|只能在 `grid-template-columns` 或 `grid-template-rows` 中使用|
|**分配剩余空间**|在所有固定尺寸（如 `px`、`%`）分配后，剩余空间按 `fr` 比例分配|
|**响应式友好**|自动适应容器大小变化|

#### **示例代码**

```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr; /* 三列，比例 1:2:1 */
}
```

- 如果容器宽度为 `400px`：
    - 第一列 = `100px`（1/4）
    - 第二列 = `200px`（2/4）
    - 第三列 = `100px`（1/4）

#### **与其他单位的对比**

```css
grid-template-columns: 100px 1fr 2fr;
```

- `100px`：固定宽度
- `1fr` 和 `2fr`：剩余空间按 1:2 分配

---

### **2. `rem` 单位（Root EM）**

#### **基本概念**

- **`rem`** 是相对于**根元素（`<html>`）字体大小**的单位。
- 1rem = 根元素的 `font-size` 值（默认通常为 `16px`）。

#### **核心特性**

|特性|说明|
|---|---|
|**相对于根元素**|不受父元素字体大小影响|
|**一致性**|全站统一缩放|
|**响应式关键**|通过修改根字体大小实现全局缩放|

#### **示例代码**

```css
html {
  font-size: 16px; /* 默认（1rem = 16px） */
}

.box {
  font-size: 1rem;    /* 16px */
  padding: 2rem;      /* 32px */
  margin: 0.5rem;     /* 8px */
}
```

#### **与 `em` 的区别**

|单位|基准|继承影响|适用场景|
|---|---|---|---|
|`rem`|根元素字体|无|全局尺寸（布局、间距）|
|`em`|当前元素字体|受父元素影响|组件内相对缩放|

```css
.parent {
  font-size: 20px;
}

.child {
  font-size: 1em;  /* 20px（继承父级） */
  font-size: 1rem; /* 16px（始终基于根） */
}
```

---

### **3. 实际应用场景**

#### **`fr` 的典型用途**

1. **等宽网格布局**

```css
grid-template-columns: 1fr 1fr 1fr; /* 三列等宽 */
```

2. **主内容 + 侧边栏**

```css
grid-template-columns: 3fr 1fr; /* 主内容占3份，侧边栏占1份 */
```

#### **`rem` 的典型用途**

1. **响应式字体大小**

```css
@media (max-width: 768px) {
  html { font-size: 14px; } /* 全局缩小 */
}
```

2. **统一间距系统**

```cs
:root {
  --space-sm: 1rem;
  --space-md: 2rem;
}
.card {
  margin-bottom: var(--space-md);
}
```

---

### **4. 综合对比**

|单位|类型|基准|适用布局|响应式能力|
|---|---|---|---|---|
|`fr`|弹性单位|网格剩余空间|CSS Grid|⭐⭐⭐⭐⭐|
|`rem`|字体相对单位|根元素字体|全局尺寸|⭐⭐⭐⭐|
|`px`|绝对单位|像素|精确控制|⭐|
|`%`|相对单位|父容器|流动布局|⭐⭐⭐|

---

### **5. 最佳实践建议**

#### **使用 `fr` 时**

- 结合 `minmax()` 防止内容溢出：

```css
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
```

- 避免与固定宽度混用时比例失衡。

#### **使用 `rem` 时**

- 在 `:root` 或 `html` 中设置基准值：

```css
html { font-size: 62.5%; } /* 1rem = 10px（16px×62.5%） */
```

- 优先用于 `padding`、`margin`、`font-size` 等需要统一缩放的属性。

---

### **总结**

- **`fr`**：Grid 布局的"弹性尺"，按比例分配剩余空间。
- **`rem`**：基于根字体大小的"全局标尺"，实现一致性缩放。
- **选择原则**：
    - 需要弹性网格 → `fr`
    - 需要全局可控的相对单位 → `rem`

## 1.8 盒子模型

### 1.理解

CSS盒子模型(Box Model)是网页布局的基础概念，它描述了每个元素在页面上所占的空间以及如何计算这些空间。每个HTML元素都可以看作是一个矩形的"盒子"，这个盒子由内到外由四个部分组成：

1. **内容区域(Content)** - 显示实际内容（文本、图像等）的区域
    - 由`width`和`height`属性控制大小
2. **内边距(Padding)** - 内容与边框之间的透明区域
    - 使用`padding`及相关属性控制
3. **边框(Border)** - 围绕内边距和内容的边框线
    - 使用`border`及相关属性控制
4. **外边距(Margin)** - 盒子与其他元素之间的透明区域（盒子外部的空间）
    - 使用`margin`及相关属性控制

![[Pasted image 20250630212430.png]]

### 2.标准盒子模型（content-box）-默认

```css
box-sizing: content-box /* W3C盒子模型（默认） */
```

```
总宽度 = width + padding-left + padding-right + border-left + border-right + margin-left + margin-right
总高度 = height + padding-top + padding-bottom + border-top + border-bottom + margin-top + margin-bottom
```

### 3.怪异盒子模型（IE-box）

![[Pasted image 20250701144002.png]]

```css
box-sizing: border-box /* IE盒子模型 */
```

```
总宽度 = width + margin-left + margin-right
总高度 = height + margin-top + margin-bottom
```

# 2.JavaScript

## 2.1 类型转换

JavaScript 是一种**动态类型语言**，变量可以在运行时自动或手动转换类型。类型转换分为两种：

1. **隐式类型转换（自动转换）**
2. **显式类型转换（手动转换）**

---

### **1. 隐式类型转换（自动转换）**

JavaScript 在某些操作中会自动转换类型，常见场景包括：

#### **(1) 字符串拼接（`+` 运算符）**

当 `+` 的一边是字符串时，另一边会被转为字符串：

```js
console.log(1 + "2");      // "12"（数字转字符串）
console.log(true + "3");   // "true3"（布尔值转字符串）
```

#### **(2) 数学运算（`-`、`*`、`/`）**

非数字值会被尝试转为数字：

```js
console.log("10" - 2);     // 8（字符串转数字）
console.log("10" * "2");   // 20（字符串转数字）
console.log("10" / "2");   // 5
console.log("abc" - 1);    // NaN（转换失败）
```

#### **(3) 布尔上下文（逻辑判断）**

在 `if`、`||`、`&&` 等逻辑判断中，值会被隐式转为布尔值：

```js
if ("hello") { }       // true（非空字符串为真）
if (0) { }             // false（0 为假）
console.log(!!"hi");   // true（双非运算符强制转布尔）
```

#### **(4) 宽松相等（`==`）**

`==` 会触发隐式类型转换（而 `===` 不会）：

```js
console.log(1 == "1");    // true（字符串转数字）
console.log(true == 1);   // true（布尔值转数字）
console.log(null == undefined); // true（特殊规则）
```

---

### **2. 显式类型转换（手动转换）**

开发者可以主动调用方法或函数转换类型。

#### **(1) 转字符串 `String()` 或 `toString()`**

```js
let num = 123;
console.log(String(num));    // "123"
console.log(num.toString()); // "123"（注意：null/undefined 不能用）

let bool = true;
console.log(bool.toString()); // "true"
```

#### **(2) 转数字 `Number()`、`parseInt()`、`parseFloat()`**

```js
console.log(Number("123"));    // 123
console.log(Number("123abc")); // NaN（无法解析）

console.log(parseInt("123px")); // 123（提取整数部分）
console.log(parseFloat("12.3")); // 12.3
```

#### **(3) 转布尔值 `Boolean()`**

```js
console.log(Boolean(1));       // true
console.log(Boolean(0));       // false
console.log(Boolean(""));      // false（空字符串为假）
console.log(Boolean("hello")); // true
```

#### **(4) 其他转换技巧**

```js
// 快速转数字
console.log(+"123"); // 123（一元加号）

// 快速转布尔值
console.log(!!"hello"); // true（双非运算符）

// 转整数（位运算技巧）
console.log(~~12.3);    // 12（双按位非）
console.log(12.3 | 0);  // 12（按位或 0）
```

---

### **3. 特殊类型转换规则**

#### **(1) Falsy 值（转为 `false` 的值）**

以下值在布尔上下文中会被转为 `false`：

```js
false, 0, "", null, undefined, NaN
```

其他所有值均为 `true`。

#### **(2) `null` 和 `undefined` 的转换**

```js
console.log(Number(null));      // 0
console.log(Number(undefined)); // NaN

console.log(String(null));      // "null"
console.log(String(undefined)); // "undefined"
```

#### **(3) 对象转原始值**

对象会先调用 `valueOf()`，再调用 `toString()`：

```js
let obj = {
  valueOf() { return 42; },
  toString() { return "Hello"; }
};
console.log(Number(obj)); // 42（优先 valueOf）
console.log(String(obj)); // "Hello"（优先 toString）
```

---

### **4. 避免类型转换的坑**

#### **(1) 使用 `===` 代替 `==`**

```js
console.log(0 == false);    // true（隐式转换）
console.log(0 === false);   // false（严格相等）
```

#### **(2) 注意 `+` 运算符的歧义**

```js
console.log(1 + 2 + "3");   // "33"（先计算 1+2，再拼接）
console.log("1" + 2 + 3);   // "123"（全部转为字符串）
```

#### **(3) 谨慎处理 `NaN`**

```js
console.log(NaN === NaN);    // false（NaN 不等于自身）
console.log(isNaN("abc"));   // true（全局 isNaN 会先转换）
console.log(Number.isNaN("abc")); // false（ES6 更安全）
```

---

### **5. 类型转换总结表**

|原始值|转字符串|转数字|转布尔值|
|---|---|---|---|
|`"123"`|"123"|123|`true`|
|`""`|""|0|`false`|
|`"abc"`|"abc"|`NaN`|`true`|
|`true`|"true"|1|`true`|
|`false`|"false"|0|`false`|
|`null`|"null"|0|`false`|
|`undefined`|"undefined"|`NaN`|`false`|
|`[]`|""|0|`true`|
|`{}`|"[object Object]"|`NaN`|`true`|

---

### **6. 最佳实践**

1. **显式转换优于隐式转换**：使用 `Number()`、`String()` 等明确意图。
2. **优先用 `===`**：避免 `==` 的隐式转换陷阱。
3. **处理边界值**：检查 `NaN`、`null`、`undefined` 等特殊情况。

## 2.2 原型链

### **1. 什么是原型链？**

原型链（Prototype Chain）是 JavaScript 实现继承的核心机制。每个对象都有一个内部属性 `[[Prototype]]`（可通过 `__proto__` 或 `Object.getPrototypeOf()` 访问），指向它的**原型对象（Prototype）**。当访问对象的属性或方法时，如果对象本身没有该属性，JavaScript 会沿着原型链向上查找，直到找到该属性或到达原型链顶端（`null`）。

---

### **2. 原型链的结构**

#### **(1) 构造函数、实例和原型的关系**

- **构造函数（Constructor）**：用于创建对象的函数（如 `function Person() {}`）。
- **实例（Instance）**：由构造函数生成的对象（如 `const p = new Person()`）。
- **原型对象（Prototype）**：每个构造函数都有一个 `prototype` 属性，指向它的原型对象。实例的 `__proto__` 指向构造函数的 `prototype`。

```js
function Person(name) {
  this.name = name;
}

// 在原型上添加方法
Person.prototype.sayHello = function() {
  console.log(`Hello, ${this.name}!`);
};

const p = new Person("Alice");

// 实例 p 的原型链：
// p -> Person.prototype -> Object.prototype -> null
```

#### **(2) 原型链查找过程**

当访问 `p.sayHello()` 时：

1. 先在 `p` 自身查找 `sayHello`（没有）。
2. 沿着 `p.__proto__`（即 `Person.prototype`）查找，找到 `sayHello` 并调用。
3. 如果还没找到，继续沿着 `Person.prototype.__proto__`（即 `Object.prototype`）查找。
4. 最终如果没找到，返回 `undefined`。

---

### **3. 原型链的终点**

所有对象的原型链最终都会指向 `Object.prototype`，而 `Object.prototype.__proto__` 是 `null`，表示原型链的终点。

```js
console.log(p.__proto__ === Person.prototype);           // true
console.log(Person.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__);                 // null
```

---

### **4. 修改原型链**

#### **(1) 直接修改 `__proto__`（不推荐）**

```js
const obj1 = { a: 1 };
const obj2 = { b: 2 };

obj1.__proto__ = obj2;  // 修改原型链
console.log(obj1.b);    // 2（从 obj2 继承）
```

**⚠️ 注意**：`__proto__` 是非标准属性，建议使用 `Object.setPrototypeOf()`。

#### **(2) 使用 `Object.create()`**

```js
const parent = { name: "Parent" };
const child = Object.create(parent); // child.__proto__ = parent
console.log(child.name); // "Parent"（继承自 parent）
```

#### **(3) 使用 `Object.setPrototypeOf()`（ES6）**

```js
const objA = { x: 1 };
const objB = { y: 2 };

Object.setPrototypeOf(objA, objB); // objA.__proto__ = objB
console.log(objA.y); // 2（从 objB 继承）
```

---

### **5. 原型链的应用（继承）**

#### **(1) 构造函数继承**

```js
function Animal(name) {
  this.name = name;
}

// Animal.prototype 是它的原型对象，所有 Animal 实例共享的方法
Animal.prototype.eat = function() {
  console.log(`${this.name} is eating.`);
};

function Dog(name, breed) {
  Animal.call(this, name); // 调用父类构造函数
  this.breed = breed;
}

// 设置 Dog.prototype 的原型为 Animal.prototype，即让 Dog 的原型对象继承 Animal 的原型对象
Dog.prototype = Object.create(Animal.prototype);
// 由于 Dog.prototype = Object.create(…) 会覆盖原有的 Dog.prototype ，导致 constructor 丢失
Dog.prototype.constructor = Dog; // 修复 constructor 指向

const dog = new Dog("Buddy", "Golden Retriever");
dog.eat(); // "Buddy is eating."
```

##### 注意事项

（1）当执行 `new Animal("Cat")` 时：
- 创建一个新对象 `{}`。
- 它的 `__proto__` 指向 `Animal.prototype`。
-  `this` 指向这个新对象，并设置 `this.name = name`。

（2）**`Object.create(Animal.prototype)`**

- 直接引用：直接 `Dog.prototype = Animal.prototype` 会导致：
    - 修改 `Dog.prototype` 也会影响 `Animal.prototype`（不符合继承逻辑），导致原型链断裂。
- `Object.create` 创建一个新对象，隔离父子类的原型。

##### 原型链关系图

```
dog (实例)
│
├── name: "Buddy"          (来自 Animal 构造函数)
├── breed: "Golden Retriever" (来自 Dog 构造函数)
└── __proto__: Dog.prototype
    │
    ├── constructor: Dog   (修复后的构造函数)
    └── __proto__: Animal.prototype
        │
        ├── eat: function  (共享方法)
        └── __proto__: Object.prototype
            │
            └── …        (toString 等默认方法)
                │
                └── __proto__: null
```

#### **(2) `class` 语法（ES6，本质仍是原型链）**

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  eat() {
    console.log(`${this.name} is eating.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // 调用父类构造函数
    this.breed = breed;
  }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.eat(); // "Buddy is eating."
```

##### 注意事项

（1）`class` 是**语法糖**，底层仍然是**原型链继承**。

（2）`extends` 和 `super` 自动处理了原型链和构造函数调用。

---

### **6. 总结**

|特性|说明|
|---|---|
|**原型（`prototype`）**|构造函数的一个属性，指向原型对象|
|**`__proto__`**|实例的内部属性，指向构造函数的 `prototype`|
|**原型链查找**|对象自身 → 原型 → 原型的原型 → … → `null`|
|**继承方式**|原型链继承、构造函数继承、`class` 语法|
|**终点**|`Object.prototype.__proto__ === null`|

原型链是 JavaScript 面向对象的核心机制，理解它有助于掌握继承、`new` 操作符、`instanceof` 等概念。

## 2.3 闭包

### 1. 定义

**闭包（Closure）** 是指一个函数能够**访问并记住其词法作用域（Lexical Scope）**，即使该函数在其词法作用域之外执行。  
简单来说：**函数嵌套函数，内部函数可以访问外部函数的变量**，即使外部函数已经执行完毕。

#### **闭包的核心特点**

1. **函数嵌套函数**（外部函数 + 内部函数）。
2. **内部函数引用外部函数的变量**。
3. **外部函数的变量不会被垃圾回收**（即使外部函数已执行完）。

---

### **2. 闭包的经典示例**

#### **示例 1：计数器**

```js
function createCounter() {
  let count = 0; // 外部函数的变量

  return function() {
    count++; // 内部函数访问外部变量
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

**解释**：

- `createCounter()` 执行后返回一个内部函数。
- 内部函数持有对外部变量 `count` 的引用，因此 `count` 不会被销毁。
- 每次调用 `counter()` 都会修改并返回更新后的 `count`。

#### **示例 2：私有变量**

```js
function createPerson(name) {
  let privateAge = 0; // 私有变量

  return {
    getName: () => name,
    getAge: () => privateAge,
    setAge: (age) => { privateAge = age; }
  };
}

const person = createPerson("Alice");
console.log(person.getName()); // "Alice"
person.setAge(25);
console.log(person.getAge()); // 25
```

**解释**：

- `privateAge` 是外部函数的局部变量，外部无法直接访问。
- 通过闭包提供的 `getAge` 和 `setAge` 方法，可以安全地读写私有变量。

---

### **3. 闭包的常见应用场景**

#### **(1) 数据封装（模拟私有变量）**

```js
function createBankAccount(balance) {
  return {
    deposit: (amount) => { balance += amount; },
    withdraw: (amount) => { balance -= amount; },
    getBalance: () => balance
  };
}

const account = createBankAccount(1000);
account.deposit(500);
console.log(account.getBalance()); // 1500
```

#### **(2) 函数工厂（动态生成函数）**

```js
function multiplyBy(factor) {
  return function(num) {
    return num * factor;
  };
}

const double = multiplyBy(2);
console.log(double(5)); // 10

const triple = multiplyBy(3);
console.log(triple(5)); // 15
```

#### **(3) 事件处理（保留上下文）**

```js
function setupButton(buttonId) {
  const button = document.getElementById(buttonId);
  let clicks = 0;

  button.addEventListener("click", function() {
    clicks++;
    console.log(`Button clicked ${clicks} times`);
  });
}

setupButton("myButton");
```


**解释**：  
每次点击按钮时，回调函数都能访问并更新 `clicks`，因为闭包保留了 `clicks` 的引用。

---

### **4. 闭包的内存问题与解决**

#### **问题：内存泄漏**

闭包会导致外部函数的变量长期驻留内存，如果不及时释放，可能引发内存泄漏。

#### **解决方案**

1. **手动解除引用**：

```js
const counter = createCounter();
counter(); // 使用后置为 null
counter = null; // 释放闭包
```

2. **避免不必要的闭包**：只在需要封装数据时使用。

---

### **5. 闭包与作用域链**

闭包的本质是**作用域链的延续**：

1. 函数定义时，会绑定当前的词法作用域。
2. 即使函数在其他作用域调用，仍能访问原始作用域的变量。

```js
function outer() {
  const x = 10;
  function inner() {
    console.log(x); // 访问外部函数的 x
  }
  return inner;
}

const innerFunc = outer();
innerFunc(); // 10（仍能访问 x）
```

---

### **6. 面试常见问题**

#### **问题 1：以下代码输出什么？**

```js
for (var i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
```

**答案**：输出 `4, 4, 4`（因 `var` 无块级作用域，闭包捕获的是最终的 `i`）。  
**修复方法**（用 `let` 或 IIFE）：

```js
// 方法 1：使用 let（块级作用域）
for (let i = 1; i <= 3; i++) {
  setTimeout(() => console.log(i), 1000);
}

// 方法 2：IIFE 创建闭包
for (var i = 1; i <= 3; i++) {
  (function(j) {
    setTimeout(() => console.log(j), 1000);
  })(i);
}
```

#### **问题 2：闭包和普通函数的区别？**

|特性|闭包|普通函数|
|---|---|---|
|**作用域**|可访问外部函数变量|只能访问自身作用域|
|**内存**|外部变量不会被回收|无持久化变量|
|**用途**|封装数据、延迟执行|独立功能单元|

---

### **7. 总结**

- **闭包 = 函数 + 其词法作用域**。
- **核心价值**：数据封装、模块化、回调函数。
- **注意事项**：避免滥用导致内存泄漏。
- **现代替代**：ES6 的 `class`（语法糖，底层仍可能用闭包）。

## 2.4 事件循环机制

**事件循环（Event Loop）** 是 JavaScript 实现**非阻塞异步编程**的核心机制，它决定了代码的执行顺序，尤其是在处理异步任务（如定时器、网络请求、I/O 操作）时。

---
### 1. 原因

JavaScript 是**单线程**语言，意味着它一次只能执行一个任务。为了避免耗时操作（如网络请求）阻塞主线程，浏览器/Node.js 使用事件循环来**调度异步任务**，保证主线程的高效运行。

---

### **2. 事件循环的核心组成部分**

#### **(1) 调用栈（Call Stack）**

- **作用**：存储同步任务的执行上下文（函数调用栈）。
- **特点**：后进先出（LIFO），同步任务立即执行。

```js
function foo() { console.log("foo"); }
function bar() { foo(); }
bar(); // 执行顺序：bar -> foo -> console.log
```

#### **(2) 任务队列（Task Queues）**

- **宏任务队列（MacroTask Queue）**：
    - 包含：`setTimeout`、`setInterval`、DOM 事件回调、I/O 操作（如 `fetch`）。
    - **特点**：每次事件循环处理一个宏任务。
- **微任务队列（MicroTask Queue）**：
    - 包含：`Promise.then`、`MutationObserver`、`queueMicrotask`。
    - **特点**：当前宏任务执行完后，立即清空所有微任务。

#### **(3) 事件循环（Event Loop）**

- **运行流程**：
    1. 从**宏任务队列**中取出一个任务执行（如 `setTimeout` 回调）。
    2. 执行过程中遇到的**微任务**（如 `Promise.then`）加入微任务队列。
    3. 当前宏任务执行完毕后，**清空微任务队列**。
    4. 如有必要，**渲染页面**（浏览器环境）。
    5. 重复上述步骤。

---

### **3. 事件循环的执行顺序**

#### **经典面试题分析**

```js
console.log("1");

setTimeout(() => {
  console.log("2");
  Promise.resolve().then(() => console.log("3"));
}, 0);

Promise.resolve().then(() => {
  console.log("4");
  setTimeout(() => console.log("5"), 0);
});

console.log("6");
```

**输出顺序**：`1 -> 6 -> 4 -> 2 -> 3 -> 5`  
**执行步骤**：
1. 同步任务：`console.log("1")` 和 `console.log("6")` 立即执行。
2. 微任务：`Promise.then` 回调（输出 `4`），并注册宏任务 `setTimeout(5)`。
3. 宏任务：第一个 `setTimeout(2)` 执行，输出 `2`，其微任务 `Promise.then(3)` 加入队列。
4. 清空微任务：执行 `console.log("3")`。
5. 下一个宏任务：执行 `setTimeout(5)`，输出 `5`。

---

### **4. 浏览器 vs Node.js 的事件循环差异**

#### **(1) 浏览器环境**

- **宏任务**：`setTimeout`、`setInterval`、`requestAnimationFrame`、I/O、UI 渲染。
- **微任务**：`Promise.then`、`MutationObserver`。

#### **(2) Node.js 环境**

- **阶段划分**（更复杂）：
    1. **Timers**：执行 `setTimeout` 和 `setInterval` 回调。
    2. **I/O Callbacks**：执行系统操作（如网络请求）的回调。
    3. **Idle/Prepare**：内部使用。
    4. **Poll**：检索新的 I/O 事件。
    5. **Check**：执行 `setImmediate` 回调。
    6. **Close Callbacks**：处理关闭事件（如 `socket.on('close')`）。
- **微任务**：`process.nextTick`（优先级高于 `Promise.then`）。

---

### **5. 关键概念对比**

|概念|宏任务（MacroTask）|微任务（MicroTask）|
|---|---|---|
|**示例**|`setTimeout`、`setInterval`|`Promise.then`、`MutationObserver`|
|**触发时机**|每次事件循环处理一个|当前宏任务结束后立即全部执行|
|**优先级**|低|高|

---

### **6. 实际应用场景**

#### **(1) 优化渲染性能**

```js
// 将耗时任务拆分到多个宏任务中
function heavyTask() {
  setTimeout(() => {
    // 分批处理数据
  }, 0);
}
```

#### **(2) 确保 DOM 更新后操作**

```js
// 微任务在 DOM 更新后触发
button.addEventListener("click", () => {
  Promise.resolve().then(() => {
    console.log("DOM updated!");
  });
});
```

#### **(3) 避免阻塞主线程**

```js
// 使用 setTimeout 将任务推迟到下一个事件循环
setTimeout(() => {
  // 长时间计算
}, 0);
```

---

### **7. 常见误区**

#### **误区 1：`setTimeout(fn, 0)` 是“立即执行”**

- 实际上，`setTimeout(fn, 0)` 是将回调加入宏任务队列，等待当前调用栈清空后执行。

#### **误区 2：微任务优先于宏任务**

- 微任务是在**当前宏任务结束后**立即执行，而非优先于所有宏任务。

---

### **8. 总结**

- **事件循环流程**：
    1. 执行同步代码（调用栈）。
    2. 处理微任务队列（全部清空）。
    3. 执行一个宏任务。
    4. 重复步骤 2-3。
- **核心规则**：
    - 同步任务 > 微任务 > 宏任务。
    - 微任务会在当前宏任务结束后**立即执行**。
- **掌握事件循环**，能避免异步编程中的执行顺序问题，写出更高效的代码！

## 2.5 异步编程Promise

### **1. 异步编程的背景**

JavaScript 是单线程语言，但通过**异步编程**可以避免阻塞主线程。传统的异步方案是**回调函数（Callback）**，但会导致"回调地狱"（Callback Hell）。ES6 引入 **Promise**，ES2017 又推出 **async/await**，让异步代码更易读写。

### 2.什么是 Promise？

Promise 是一个**表示异步操作最终完成或失败的对象**，它有三种状态：
- **Pending（进行中）**：初始状态。
- **Fulfilled（已成功）**：操作成功完成。
- **Rejected（已失败）**：操作失败。

### 3.创建 Promise

```js
const promise = new Promise((resolve, reject) => {
  // 异步操作（如 AJAX、setTimeout）
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve("操作成功！"); // 状态变为 Fulfilled
    } else {
      reject("操作失败！");  // 状态变为 Rejected
    }
  }, 1000);
});
```

### **4.使用 Promise**

通过 `.then()` 处理成功，`.catch()` 处理失败：

```js
promise
  .then((result) => {
    console.log(result); // "操作成功！"
  })
  .catch((error) => {
    console.error(error); // "操作失败！"
  });
```

#### **5.Promise 链式调用**

```js
fetch("/api/data")           // 返回 Promise
  .then((response) => response.json()) // 解析 JSON
  .then((data) => console.log(data))   // 处理数据
  .catch((error) => console.error(error)); // 统一错误处理
```

### **6.Promise 静态方法**

|方法|说明|
|---|---|
|`Promise.resolve(value)`|返回一个已成功的 Promise|
|`Promise.reject(error)`|返回一个已失败的 Promise|
|`Promise.all([p1, p2])`|所有 Promise 成功时才成功|
|`Promise.race([p1, p2])`|第一个完成的 Promise 决定结果|

## 2.6 异步编程async/await

async/await 是基于 Promise 的语法糖。

### **1.定义**

- `async`：声明一个函数是异步的，返回值会被自动包装成 Promise。
- `await`：等待 Promise 完成，并返回其结果（只能在 `async` 函数中使用）。

### **2.基本用法**

```js
async function fetchData() {
  try {
    const response = await fetch("/api/data"); // 等待 Promise 完成
    const data = await response.json();       // 等待 JSON 解析
    console.log(data);
  } catch (error) {
    console.error(error); // 统一捕获错误
  }
}

fetchData(); // 调用 async 函数
```

### **3.async/await 的优势**

1. **代码更线性**：避免回调嵌套，类似同步代码的写法。
2. **错误处理更直观**：直接用 `try/catch` 捕获错误。
3. **调试更方便**：可以在 `await` 处设置断点。

### **4.注意事项**

- `await` 会阻塞当前 `async` 函数内的代码，但不会阻塞主线程。
- 并行任务可以用 `Promise.all`
```js
async function fetchAll() {
  const [data1, data2] = await Promise.all([
	fetch("/api/data1").then((res) => res.json()),
	fetch("/api/data2").then((res) => res.json()),
  ]);
  console.log(data1, data2);
}
```

---

### **5. Promise 和 async/await 对比**

|特性|Promise|async/await|
|---|---|---|
|**代码风格**|链式调用（`.then()`）|同步式写法|
|**错误处理**|`.catch()`|`try/catch`|
|**调试**|较难跟踪|更直观|
|**适用场景**|简单异步逻辑|复杂异步流程|

- **Promise**：用 `.then()` 和 `.catch()` 处理异步操作，适合简单场景。
- **async/await**：基于 Promise，让异步代码更像同步代码，适合复杂逻辑。
- **核心原则**：
    - **异步非阻塞**：JavaScript 通过事件循环处理异步任务。
    - **错误优先**：始终用 `.catch()` 或 `try/catch` 处理错误。

### 6.经典面试题

#### **问题：以下代码的输出顺序是什么？**

```js
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

async function foo() {
  console.log("4");
  await Promise.resolve();
  console.log("5");
}

foo();
console.log("6");
```

**答案**：`1 -> 4 -> 6 -> 3 -> 5 -> 2`  
**解析**：
1. 同步代码：`1`, `4`, `6`。
2. 微任务（`Promise.then` 和 `await`）：`3`, `5`。
3. 宏任务（`setTimeout`）：`2`。

### 7.为什么 async 和 await 必须一起使用？

`async` 和 `await` 是 JavaScript 异步编程中**密不可分的组合**。

#### **(1) `async` 的作用**

##### **标记函数为异步**

- `async` 关键字声明一个函数是**异步函数**，它的返回值会被**自动包装成 Promise**。
- 即使函数内没有 `await`，`async` 也会强制返回 Promise：

```js
async function foo() {
  return 42;
}
console.log(foo()); // Promise {<fulfilled>: 42}
```

##### **允许使用 `await`**

- **`await` 只能在 `async` 函数内部使用**，否则会报语法错误：

```js
function bar() {
  await Promise.resolve(); // SyntaxError: await is only valid in async function
}
```

---

#### **(2) `await` 的作用**

##### **暂停异步函数执行**

- `await` 会**暂停当前 `async` 函数的执行**，等待右侧的 Promise 完成，并返回其结果。
- 它让异步代码的写法更像同步代码：

```js
async function fetchData() {
  const response = await fetch("/api/data"); // 等待网络请求
  const data = await response.json();       // 等待 JSON 解析
  return data;
}
```

##### **错误处理**

- 如果 `await` 的 Promise 被拒绝（rejected），会抛出异常，可以用 `try/catch` 捕获：

```js
async function loadData() {
  try {
	const data = await fetch("/api/invalid-url");
  } catch (error) {
	console.error("请求失败:", error);
  }
}
```

---

#### **(3)为什么必须配合使用？**

|场景|单独使用 `async`|单独使用 `await`|`async` + `await`|
|---|---|---|---|
|**返回值**|自动包装为 Promise|语法错误|正常返回 Promise|
|**执行控制**|无暂停效果|无法使用（语法错误）|可暂停函数执行|
|**代码可读性**|无优势|不可用|异步代码像同步代码|

##### **关键原因**

1. **语法规则**：JavaScript 引擎规定 `await` 必须位于 `async` 函数内。
2. **执行控制**：`async` 提供异步上下文，`await` 提供暂停能力。
3. **错误处理**：`async` 函数内才能用 `try/catch` 捕获 `await` 的错误。

---

#### **(4)常见问题**

##### **Q1：能否在全局作用域使用 `await`？**

- **不能**。但现代浏览器和 Node.js 支持**顶层 `await`（Top-Level Await）**：

```js
// 在模块中可行（ES2022+）
const data = await fetch("/api/data");
```

##### **Q2：`async` 函数可以没有 `await` 吗？**

- **可以**，但通常无意义（除非需要强制返回 Promise）：

```js
async function useless() {
  return 123; // 等价于 Promise.resolve(123)
}
```

##### **Q3：为什么不用 `Promise.then` 代替 `await`？**

- `await` 的优势：
    - **代码更线性**，避免回调地狱。
    - **错误处理更直观**（`try/catch` vs `.catch()`）。
    - **调试更方便**（断点可停在 `await` 行）。

---

#### **(5)总结**

- **`async`**：声明函数为异步，允许使用 `await`，并自动包装返回值为 Promise。
- **`await`**：暂停异步函数执行，等待 Promise 完成，使异步代码更像同步写法。
- **必须一起使用**：
    - 这是 JavaScript 的语法规则。
    - 组合后能实现**非阻塞异步** + **同步代码风格**的最佳实践。

##### **正确示例**

```js
async function getUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    return await response.json();
  } catch (error) {
    console.error("获取用户数据失败:", error);
    throw error; // 重新抛出错误
  }
}
```

## 2.7 ES6 新特性和语法

ES6（ECMAScript 2015）是 JavaScript 的重大更新，引入了许多现代语法和功能，显著提升了开发效率。

### 1. **变量声明：`let` 和 `const`**

`let`：块级作用域变量，可重新赋值。

```js
let x = 10;
if (true) {
  let x = 20; // 块级作用域
}
```

`const`：块级作用域常量，不可重新赋值（但对象/数组内容可修改）。

```js
const PI = 3.14;
const arr = [1, 2];
arr.push(3); // 允许
```

### 2.箭头函数（Arrow Functions）

更简洁的函数语法，自动绑定 `this`（继承父作用域）。

```js
const add = (a, b) => a + b;
const greet = name => `Hello, ${name}!`;
```

语法形式。

```js
// 1. 单一参数（可省略括号）
const greet = name => {
  return `Hello, ${name}!`;
};

// 2. 无参数（需保留空括号）
const sayHi = () => {
  return "Hi!";
};

// 3. 多参数（需括号）
const add = (a, b) => {
  return a + b;
};

// 4. 单行表达式（隐式返回，省略大括号和 return）
const double = x => x * 2;
```

### 3.模板字符串（Template Literals）

支持多行字符串和变量插值。

```js
const name = "Alice";
const message = `Hello, ${name}! This is a multiline string.`;
```

### 4.解构赋值（Destructuring）

从数组或对象中提取值。

```js
// 数组解构
const [a, b] = [1, 2];

// 对象解构
const { name, age } = { name: "Bob", age: 30 };
```

### 5.默认参数（Default Parameters）

为函数参数设置默认值。

```js
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}
```

### 6.扩展运算符（Spread/Rest Operator）

**展开数组/对象**。

```js
const arr1 = [1, 2];
const arr2 = […arr1, 3]; // [1, 2, 3]

const obj1 = { a: 1 };
const obj2 = { …obj1, b: 2 }; // { a: 1, b: 2 }
```

**剩余参数**（收集多余参数）。

```js
function sum(…numbers) {
  return numbers.reduce((a, b) => a + b);
}
```

### 7.对象字面量增强

简写属性和方法。

```js
const name = "Alice";
const person = {
  name, // 等价于 name: name
  sayHi() { // 等价于 sayHi: function() {}
    console.log(`Hi, ${this.name}`);
  }
};
```

### 8.**Promise 和异步编程**

**`Promise`**：处理异步操作。

```js
fetch("/api/data")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

**`async/await`**（ES2017，基于 ES6 Promise）。

```js
async function fetchData() {
  try {
    const data = await fetch("/api/data");
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

### 9.**模块化（Modules）**

导出。

```js
// math.js
export const add = (a, b) => a + b;
export default function multiply(a, b) { return a * b; }
```

导入。

```js
import { add } from './math.js';
import multiply from './math.js';
```

### 10.Class 类

基于原型的语法糖，更接近传统面向对象语言。

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}
```

### 11.Map 和 Set

**`Map`**：键值对集合，键可以是任意类型。

```js
const map = new Map();
map.set("name", "Alice");
console.log(map.get("name")); // "Alice"
```

**`Set`**：唯一值的集合。

```js
const set = new Set([1, 2, 2, 3]);
console.log([…set]); // [1, 2, 3]
```

### 12.其他实用特性

**`Symbol`**：唯一且不可变的值，常用作对象键。

```js
const id = Symbol("id");
const obj = { [id]: 123 };
```

**`for…of`**：遍历可迭代对象（如数组、字符串）。

```js
for (const num of [1, 2, 3]) {
  console.log(num);
}
```

## 2.8 `const`、`var` 和 `let` 的区别

在 JavaScript 中，`const`、`var` 和 `let` 都用于声明变量，但它们有重要的区别：

### 1. 作用域 (Scope)

- **`var`**: 函数作用域 (function-scoped)

```js
if (true) {
  var x = 10;
}
console.log(x); // 10 (在if块外仍然可访问)
```

- **`let` 和 `const`**: 块级作用域 (block-scoped)

```js
if (true) {
  let y = 20;
  const z = 30;
}
console.log(y); // ReferenceError: y is not defined
console.log(z); // ReferenceError: z is not defined
```

### 2. 变量提升 (Hoisting)

- **`var`**: 会提升声明，初始值为 `undefined`

```js
console.log(a); // undefined
var a = 5;
```

- **`let` 和 `const`**: 也会提升，但在初始化前访问会报错 (暂时性死区)
	- TDZ (Temporal Dead Zone) - 暂时性死区，指变量在声明前不可访问的时期

```js
console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 5;
```

#### 2.1 核心原理

JavaScript 引擎在代码执行前会进行**编译阶段**，在这个阶段：

1. **扫描并收集**所有 `var` 声明
2. **创建变量**在作用域的最顶部
3. **初始化为 `undefined
4. **保留赋值操作**在原有位

#### 2.2 实际表现

```js
console.log(a); // 输出: undefined
var a = 5;
console.log(a); // 输出: 5
```

实际执行顺序相当于：

```js
var a = undefined; // 提升声明
console.log(a); // 输出: undefined
a = 5; // 赋值保持在原位
console.log(a); // 输出: 5
```

#### 2.3 深入理解

##### 1. 分两个阶段处理

- **编译阶段**：处理声明

```js
// 源代码
function example() {
  console.log(b);
  var b = 10;
}

// 编译后（概念上）
function example() {
  var b = undefined; // 声明提升
  console.log(b);
  b = 10; // 赋值保持原位
}
```

- **执行阶段**：按顺序执行代码

##### 2. 函数作用域提升

`var` 会提升到**当前函数作用域**的顶部：

```js
function test() {
  if (false) {
    var x = 5; // 这个声明仍会提升
  }
  console.log(x); // undefined，因为声明提升了但赋值不会执行
}
```

##### 3. 与函数提升的区别

函数声明也会提升，但行为不同：

```js
console.log(foo); // [Function: foo]
function foo() {}
console.log(bar); // undefined
var bar = function() {};
```

#### 2.4 特殊案例

##### 1.重复声明

```js
var x = 1;
var x; // 不会重置为undefined，保留原值
console.log(x); // 1
```

##### 2. 条件声明

```js
console.log(y); // undefined，不管条件如何都会提升
if (false) {
  var y = 5;
}
```

#### 2.5 与 `let/const` 的区别

`let/const` 也有提升，但存在**暂时性死区(TDZ)**：

```js
console.log(z); // ReferenceError
let z = 10;
```

#### 2.6 为什么这样设计？

1. **历史原因**：早期 JavaScript 需要快速解析
2. **函数优先**：允许函数在任何位置调用
3. **灵活性**：虽然现在被认为是设计缺陷

#### 2.7 现代实践建议

1. 使用 `let/const` 代替 `var`
2. 如果需要使用 `var`，将所有声明放在作用域顶部
3. 使用 lint 工具检测变量提升问题

```js
// 好的实践（即使使用var）
function example() {
  var a = 1;
  var b = 2;
  
  // 其他代码
}
```

### 3. 重复声明

- **`var`**: 允许重复声明

```js
var c = 1;
var c = 2; // 不会报错
```

- **`let` 和 `const`**: 不允许重复声明

```js
let d = 1;
let d = 2; // SyntaxError: Identifier 'd' has already been declared
```

### 4. 值的修改

- **`var` 和 `let`**: 可以重新赋值

```js
var e = 1;
e = 2; // 允许

let f = 1;
f = 2; // 允许
```

- **`const`**: 声明时必须**初始化**，且不能重新赋值

```js
const g = 1;
g = 2; // TypeError: Assignment to constant variable

const h; // SyntaxError: Missing initializer in const declaration

// _注意_: 对于对象和数组，`const` 只保证引用不变，内容可以修改

const obj = {a: 1};
obj.a = 2; // 允许
obj = {}; // TypeError
```


### 5.使用建议

1. 默认使用 `const` - 适用于不需要重新赋值的变量
2. 需要重新赋值时使用 `let`
3. 避免使用 `var` - 除非有特殊需求或维护旧代码
4. 使用 `const` 有助于提高代码可读性和减少错误

### 6.总结表

|特性|var|let|const|
|---|---|---|---|
|作用域|函数作用域|块级作用域|块级作用域|
|变量提升|是|是(但TDZ)|是(但TDZ)|
|重复声明|允许|不允许|不允许|
|重新赋值|允许|允许|不允许|
|初始化要求|可选|可选|必须|

## 2.9 常用函数

[[函数&方法]]

# 3.VUE 3

## 3.1 VUE 2 和 VUE 3 的区别

## 3.2 VUE 3 的常用函数

# 4.计网

## 4.1 HTTP、HTTPS

## 4.2 TCP协议

## 4.3 浏览器渲染的基本原理

## 4.4 跨域解决方案

### 1.前端

配置 `Access-Control-Allow-Origin` 方法如下。

#### 1.1 针对 `node.js` 或 `express`

```js
// service.js
const express = require("express")
const cors = require("cors") // 添加cors依赖
const app = express()

// 添加cors中间件
app.use(cors({
	origin: ["http://localhost:3000", "http://domain2.com"],
}))
```

####  1.2 针对 `VUE`

建立一个中转服务器。

![[Pasted image 20250704224324.png]]

#### 1.3 自建中转服务器

![[Pasted image 20250704224408.png]]

### 2.后端

#### 2.1 在目标方法上添加 `@CrossOrigin`

使用范围：在未添加 `Security` 模块时可使用。

#### 2.2 添加 `CROS` 过滤器

```java
/* CorsConfiguration */
@Configuration
public class CorsConfig {
	@Bean
	public CorsFilter corsFilter() {
		CorsConfiguration corsConfiguration = new CorsConfiguration();
		corsConfiguration.addAllowerdOrigin("*");
		corsConfiguration.addAllowerdHeader("*");
		corsConfiguration.addAllowerdMethod("*");
		
		UrlBasedCorConfigurationSource source = new UrlBasedCorConfigurationSource();
		source.registerCorsConfiguration("/**", corsConfiguration);
		return new CorsFilter(source);
	}
}
```

#### 2.3 拦截器：实现 `WebMvcConfigurer` 接口，重写 `addCorsMappings` 方法【最常用】

个人财务管理系统中的配置如下。

```java
/* CorsConfig.java */

package com.fm.financialmanagementsystem.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*") // 允许所有域名（生产环境应限制）
                .allowedOriginPatterns("*")  // 使用allowedOriginPatterns替代
                /*.allowedOrigins("*") // 生产环境替换为小程序域名*/
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(false) // 修改过
                .maxAge(3600); // 有效期
    }
}
```

### 3. `JsonP`

利用了 `script` 标签不会触发浏览器 CORS 机制的原理。需要服务器端和客户端一起配合，最为复杂。

### 4.增加反向代理服务器：`Nginx` 或 `Apache`

![[Pasted image 20250704235550.png]]

### 5.搭建 BFF 层

## 4.4 缓存解决方案

## 4.5 常见的Web安全攻击及其防御措施

# 5.数据结构与算法

# 6.项目

## 6.1 介绍项目的基本流程

## 6.2 为什么要在项目中使用 Pinia

Pinia是Vue的官方状态管理库。

### 1. 替代 Vuex

Pinia是Vuex的下一代替代品，具有以下优势：

- 更简单的API设计
- 完整的TypeScript支持
- 更轻量级
- 更好的开发体验

### 2. 项目中的具体用途

在这个项目中，Pinia被用来管理全局状态，特别是：

- 用户认证状态（token）
- 菜单和路由状态
- 标签页管理
- 侧边栏折叠状态
- 持久化存储

### 3. 为什么选择Pinia而不是其他方案

#### 相比于Composition API直接使用

- **集中管理**：Pinia提供了集中式的状态管理，避免状态分散在各个组件中
- **跨组件共享**：方便多个组件访问和修改同一状态
- **持久化**：项目中使用了localStorage进行状态持久化

#### 相比于Vuex

- **更简单的API**：不需要mutations，直接修改状态
- **TypeScript支持更好**：Pinia从一开始就为TS设计
- **模块化**：每个store都是自动命名空间的

### 4. 项目中的具体实现

在`stores/index.js`中，Pinia被用来：

- 管理应用的整体状态（state）
- 提供操作状态的方法（actions）
- 实现路由菜单的动态加载（addMenu方法）
- 状态持久化（通过watch和localStorage）

### 5. 与Vue Router的集成

Pinia与Vue Router配合使用，实现了：

- 路由守卫中的状态检查（检查token）
- 动态路由加载（基于权限）
- 标签页管理

### 总结

在这个项目中使用Pinia主要是因为：

1. 它提供了集中式、类型安全的状态管理解决方案
2. 简化了复杂状态的管理和跨组件共享
3. 与Vue3的Composition API完美配合
4. 提供了比Vuex更简单直观的API
5. 支持状态持久化等高级功能

Pinia帮助项目更好地组织和管理全局状态，特别是与路由、权限相关的复杂状态逻辑。