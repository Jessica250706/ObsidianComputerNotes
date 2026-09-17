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

