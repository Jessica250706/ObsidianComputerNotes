---
tags:
  - VUE3
  - 前端
beginDate: 2026-09-17
endDate:
---
# 0.参考

【黑马程序员前端 Vue 3 小兔鲜电商项目实战，vue 3 全家桶从入门到实战电商项目一套通关】https://www.bilibili.com/video/BV1Ac411K7EQ?p=24&vd_source=4e42d3c23020c1c6dc6a9aac2d11ab9c


# 1.项目起步

## 1.1 定制 Element Plus 主题

### 1.1.1 安装 Sass

### 1.1.2 准备定制化的样式文件

```scss title:'src\styles\element\index.scss'
@forward 'element-plus/theme-chalk/src/common/var.scss'
  with(
    $colors: (
      // 主色
      'primary': ('base': #27ba9b),
      // 成功色
      'success': ('base': #1dc779),
      // 警告色
      'warning': ('base': #ffb302),
      // 危险色
      'danger': ('base': #e26237),
      // 错误色
      'error': ('base': #cf4444),
    )
  );
```

### 1.1.3 自动导入配置

1. 配置 Element Plus 采用 Sass 样式配色系统
2. 自动导入定制化样式文件进行样式覆盖

