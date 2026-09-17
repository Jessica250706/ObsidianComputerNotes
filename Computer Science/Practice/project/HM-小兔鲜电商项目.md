---
tags:
  - VUE3
  - 前端
beginDate: 2026-09-17
endDate:
---
# 0.参考

【黑马程序员前端 Vue 3 小兔鲜电商项目实战，vue 3 全家桶从入门到实战电商项目一套通关】https://www.bilibili.com/video/BV1Ac411K7EQ?p=24&vd_source=4e42d3c23020c1c6dc6a9aac2d11ab9c

接口文档：[https://www.apifox.cn/apidoc/shared-c05cb8d7-e591-4d9c-aff8-11065a0ec1de/api-67132167](https://www.apifox.cn/apidoc/shared-c05cb8d7-e591-4d9c-aff8-11065a0ec1de/api-67132167)




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

```ts title:'vite.config.ts' hl:19-20,28-35
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueDevTools from 'vite-plugin-vue-devtools'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// https://vite.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    vueDevTools(),
    AutoImport({
      resolvers: [ElementPlusResolver({ importStyle: 'sass' })],
    }),
    Components({
      // 1. 配置 Element Plus 采用 sass 样式配色系统
      resolvers: [ElementPlusResolver({ importStyle: 'sass' })],
    }),
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url)),
    },
  },
  css: {
    preprocessorOptions: {
      scss: {
        // 2. 自动导入定制化样式文件进行样式覆盖
        additionalData: `@use "@/styles/element/index.scss" as *;`,
      },
    },
  },
})
```

注意，需要按需引入，全局引入样式不生效。

```ts title:main.ts
import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus'
// import 'element-plus/dist/index.css'
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
import router from '@/router'
import { createPinia } from 'pinia'
import { createPersistedState } from 'pinia-plugin-persistedstate'
import { zhCn } from 'element-plus/es/locales.mjs'

const app = createApp(App)
const pinia = createPinia()
const persist = createPersistedState()
pinia.use(persist)
app.use(pinia)
app.use(router)
// app.use(ElementPlus, {
//   locale: zhCn,
// })
app.mount('#app')
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

# 2.首页

## 2.1 图片懒加载插件

插件实现。

```ts title:'src\directives\lazy.ts'
import type { App, DirectiveBinding } from 'vue'
import { useIntersectionObserver } from '@vueuse/core'

// 定义懒加载插件
export const lazyPlugin = {
  install(app: App) {
    // 定义全局指令
    app.directive('img-lazy', {
      mounted(el: HTMLImageElement, binding: DirectiveBinding<string>) {
        // el：指令绑定的那个元素 img
        // binding.value 指令等于号后面绑定的表达式的值 图片url

        const { stop } = useIntersectionObserver(el, ([entry]) => {
          if (entry?.isIntersecting) {
            // 进入视口区域
            el.src = binding.value
            stop()
          }
        })
      },
    })
  },
}
```

全局导入。

```ts title:'src\main.ts' hl:8,19
import { createApp } from 'vue'
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
// import ElementPlus from 'element-plus'
// import 'element-plus/dist/index.css'
// import { zhCn } from 'element-plus/es/locales.mjs'
import { createPinia } from 'pinia'
import { createPersistedState } from 'pinia-plugin-persistedstate'
import { lazyPlugin } from '@/directives/lazy.ts'
import router from '@/router'
import App from './App.vue'
import '@/styles/common.scss'

const app = createApp(App)
const pinia = createPinia()
const persist = createPersistedState()
pinia.use(persist)
app.use(pinia)
app.use(router)
app.use(lazyPlugin)
// app.use(ElementPlus, {
//   locale: zhCn,
// })
app.mount('#app')
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

使用，把 `:src="item.picture"` 改成 `v-img-lazy="item.picture"`。

```vue title:'src\views\Home\components\HomeNew.vue' hl:7
<template>
  <HomePanel title="新鲜好物" sub-title="新鲜出炉 品质靠谱">
    <template #main>
      <ul class="goods-list">
        <li v-for="item in newList" :key="item.id">
          <RouterLink :to="`/detail/${item.id}`">
            <img v-img-lazy="item.picture" alt="" />
            <p class="name">{{ item.name }}</p>
            <p class="price">&yen;{{ item.price }}</p>
          </RouterLink>
        </li>
      </ul>
    </template>
  </HomePanel>
</template>
```

