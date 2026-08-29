---
tags:
  - 前端
  - VUE3
  - ElementPlus
  - el-dropdown
  - BLOG
date: 2025-09-01
---
标题：【Vue3+Element Plus】修改el-dropdown（下拉菜单）的默认样式

博主采用了网上的方法，代码如下：

```vue
<script lang="ts" setup>
// 操作类型定义
type Operation = 'STOP' | 'RESTART' | 'RESET' | 'START' | 'DELETE'

const props = defineProps<Props>()

// 操作映射配置
const OPERATION_CONFIG: Record<Operation, { label: string; color: string }> = {
  STOP: { label: '关闭', color: '#303133' },
  RESTART: { label: '重启', color: '#303133' },
  RESET: { label: '重置', color: '#303133' },
  START: { label: '开启', color: '#303133' },
  DELETE: { label: '移除', color: '#F92022' },
}
</script>

<template>
  <div class="inline-flex size-[16px]">
    <el-dropdown
      trigger="click"
    >
      <!-- 更多按钮-pic -->
      <img
        :src="moreImg"
        class="size-[16px] cursor-pointer"
        alt="More options"
      />
      <!-- 下拉菜单 -->
      <template #dropdown>
        <el-dropdown-menu class="menu-container">
          <el-dropdown-item
            v-for="item in props.fatherMessage.actionList"
            :key="item.name"
            :style="{
              '--item-color': item.name ? OPERATION_CONFIG[item.name].color : '#303133',
              '--hover-color': item.name === 'DELETE'
                ? OPERATION_CONFIG[item.name].color
                : '#1E5EFF',
            }"
            class="command-item"
            @click="handleCommand(item.name)"
          >
            {{ OPERATION_CONFIG[item.name].label }}
          </el-dropdown-item>
        </el-dropdown-menu>
      </template>
    </el-dropdown>
  </div>
</template>

<style lang="scss" scoped>
:deep(.el-popper .menu-container) {
  width: 86px !important;
  background: transparent !important; // 背景色移到外层
  box-shadow: none !important; // 阴影移到外层
  border: none !important;
}

:deep(.el-popper.el-dropdown__popper) {
  background-color: #FFFFFF !important;
  box-shadow: 0px 0px 6px rgba(30,94,255,0.12) !important;
  border: 1px solid #E4E7ED !important;
}

:deep(.el-dropdown-menu__item.command-item) {
  padding: 1px 22px !important;
  border-radius: 4px 4px 4px 4px !important;
  margin: 2.5px 9px !important;
  width: 86px !important;

  font-size: 12px !important;
  font-weight: 400 !important;
  line-height: 20px !important;

  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  text-align: center !important;

  color: var(--item-color) !important;
  background-color: #FFFFFF !important;

  // 悬停状态
  &:hover {
    color: var(--hover-color) !important;
    background-color: #F9F9F9 !important;
  }

  // 强制覆盖Element的激活状态样式
  &.is-active,
  &:focus {
    color: var(--hover-color) !important;
    background-color: #F9F9F9 !important;
  }
}
</style>
```

效果如下。

修改前：

修改后：