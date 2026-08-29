---
tags:
  - 前端
  - VUE3
  - ElementPlus
  - el-table
  - BLOG
date: 2025-08-22
---
标题：【Vue3+Element Plus】修改el-table树形结构的默认箭头样式

博主一开始是采取了网上的方法，使用了以下的代码：

```SCSS
::v-deep .el-icon-arrow-right {
  color: #49c0ff;
}
 
::v-deep.el-table .el-table__expand-icon {
  .el-icon-arrow-right:before {
    content: "\e791";
  }
}
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

且在修改为Vue3支持的格式后，发现这段代码无效。

随后博主找到了现在的Element Plus中的icon部分，发现现在的右向实心箭头和以前的版本不同。

![[屏幕截图 2025-08-22 140738.png]]

博主在网页修改了el-table中的箭头的d，发现这一方法确实可以改变箭头，但无法在vue文件中对svg的path的d进行改变。（如果有大佬知道怎么用这个改可以发个评论吗？）

​![[屏幕截图 2025-08-22 135450.png]]

```html
<path 
    fill="currentColor"
    d="M340.864 149.312a30.592 30.592 0 0 0 0 42.752L652.736 512 340.864 831.872a30.592 30.592 0 0 0 0 42.752 29.12 29.12 0 0 0 41.728 0L714.24 534.336a32 32 0 0 0 0-44.672L382.592 149.376a29.12 29.12 0 0 0-41.728 0z" 
    style="d: path(&quot;M 384 192 v 640 l 384 -320.064 Z&quot;);">
</path>
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

然后，博主再尝试了插槽也失败后，经过一番探索，决定隐藏原生的箭头，并自己画一个。

博主的目标是在一个正方形中画出以下两个箭头。

1. 向右的未展开箭头

![[屏幕截图 2025-08-22 142343.png|163]]

2. 向下的展开箭头

![[屏幕截图 2025-08-22 142513.png|171]]

但是，事实上，在展开时这个箭头会自动逆时针旋转90°，所以其实只需要画两个向右箭头就可以了。

至于如何画出这个箭头，博主使用了clip-path: polygon()进行绘制，其规则如下：

```html
polygon(x1 y1, x2 y2, x3 y3, x4 y4, xn yn)
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

最后的实现代码如下：

```css
<style scoped>
/* 箭头部分 */
:deep(.el-table .el-table__expand-icon) {
  display: inline-block;
  width: 0;
  height: 0;
  border-style: solid;
  cursor: pointer;
}

/* 隐藏原有的箭头 */
:deep(.el-table .el-table__expand-icon .el-icon) {
  display: none;
}

/* 未展开状态：向右的三角形 */
:deep(.el-table .el-table__expand-icon:not(.el-table__expand-icon--expanded)) {
  border-width: 6px;
  border-color: #606266;
  clip-path: polygon(25% 0, 25% 100%, 75% 50%);
}

/* 展开状态：向下的三角形 */
:deep(.el-table .el-table__expand-icon--expanded) {
  border-width: 6px;
  border-color: #606266;
  clip-path: polygon(25% 0, 25% 100%, 75% 50%);
}
</style>
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

效果如下图。

未展开：

![[屏幕截图 2025-08-22 142901.png]]

展开：

![[屏幕截图 2025-08-22 142916.png]]

