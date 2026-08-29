---
tags:
  - 前端
  - VUE3
  - Vite
date: 2025-09-17
---
本项目基于 Vue 3+TS，使用 pnpm。故需先下载 pnpm，教程见网络。也可替换 pnpm 为 npm。若替换为 npm，则部分安装依赖语句需要更换语法。

# 1.使用 Vite 创建项目

打开需要创建项目的大文件夹。

![[Pasted image 20250917085734.png]]

打开终端，输入 `pnpm create vite@latest`，根据提示输入项目名称，选择框架。

![[Pasted image 20250917085921.png]]

再选择语言。

![[Pasted image 20250917085952.png]]

根据提示语切换到刚创建的文件夹，并进行初始化。

![[Pasted image 20250917090052.png]]

![[Pasted image 20250917090132.png]]

![[Pasted image 20250917090202.png]]

点击连接，显示页面如下。

![[Pasted image 20250917091353.png]]

# 2.安装路由

打开刚才创建的文件，本文使用 VSCode 打开。

## 2.1 配置别名

### 2.1.1 `vite.config.js` 配置文件，添加如下配置

```ts
server: {
	host: '0.0.0.0', // 任何ip都可以访问
	port: 8080, // 项目端口号
	hmr: true, //开启热加载
	open: true, //自动打开浏览器
},
```

![[Pasted image 20250917094135.png]]

### 2.1.2 vite 配置别名

在项目的终端安装必须的依赖。

```shell
pnpm install @types/node --save-dev
```

![[Pasted image 20250917093644.png]]

在 `vite.config.ts` 中添加代码。

```ts
import { resolve } from 'path'

resolve: {
	alias: [
	  {
		find: '@',
		replacement: resolve(__dirname, 'src')
	  }
	]
},
```

![[Pasted image 20250917100617.png]]

### 2.1.3 添加 baseUrl 和 paths

`tsconfig.json` 里面添加如下代码。

```json
"compilerOptions": {
   "baseUrl": ".",
   "paths": {
     "@/*": ["src/*"]
   },
 },
```

![[Pasted image 20250917101828.png]]

若 `tsconfig.json` 文件中存在 `{ "path": "./tsconfig.app.json" }` ，则在 `tsconfig.app.json` 文件中也添加如下代码。

```json
"baseUrl": ".",
"paths": {
  "@/*": ["src/*"]
},
```

![[Pasted image 20250917162058.png]]

#### 拓展：了解 `tsconfig.app.json` 和 `tsconfig.node.json` 的不同

| 特性       | `tsconfig.app.json` | `tsconfig.node.json`               |
| -------- | ------------------- | ---------------------------------- |
| **目标环境** | 浏览器环境               | Node.js 环境                         |
| **编译目标** | ES2020 / ESNext     | ES2022                             |
| **模块系统** | ES Module           | CommonJS / ES Module               |
| **用途**   | 前端应用代码              | Vite 配置和构建脚本                       |
| **包含文件** | `src/**/*`          | `vite.config.ts`, `package.json` 等 |
| **输出目录** | `dist`              | 无（通常不输出）                           |
##### 1. `tsconfig.app.json` - 前端应用配置

**特点**：针对浏览器环境优化，支持 Vue 文件，严格类型检查。

##### 2. `tsconfig.node.json` - Node.js 环境配置

**特点**：针对 Node.js 环境，用于类型检查构建配置文件。

## 2.2 安装路由

### 2.2.1 安装依赖

```shell
pnpm install vue-router@4
# pnpm add vue-router@4
```

![[Pasted image 20250917102025.png]]

### 2.2.2 新建路由文件

在 src 目录下新建 router 文件夹，然后新建 `index.ts` 文件

```ts
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'
import Layout from '@/components/HelloWorld.vue'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/home',
    name: 'Home',
    component: Layout,
  },
]

const router = createRouter({
  history: createWebHistory(),
  routes,
  strict: false,
  // 切换页面，滚动到最顶部
  scrollBehavior: () => ({ left: 0, top: 0 }),
})

export default router
```

![[Pasted image 20250917162812.png]]

### 2.2.3 在 `main.ts` 里面引入路由

```ts
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router/index'

const app = createApp(App)

app.use(router)
app.mount('#app')
```

![[Pasted image 20250917103531.png]]

### 2.2.4 修改 `App.vue` 和 `HelloWorld.vue`

修改 `App.vue` 内代码为

```vue
<template>
  <router-view/>
</template>
```

![[Pasted image 20250917104225.png]]

修改 `HelloWorld.vue` 内代码为

```vue
<script setup lang="ts">

</script>

<template>
  <div>Hello World</div>
</template>

<style lang="scss" scoped>

</style>
```

![[Pasted image 20250917104254.png]]

### 2.2.5 运行项目，检验结果

在终端输入。

```shell
pnpm run dev
```

![[Pasted image 20250917104508.png]]

若无法正确查看页面，可在终端输入 `pnpm install vite.config.ts` 重启后则可正常显示。

![[屏幕截图 2025-09-17 111854.png]]

最终页面显示如下：

![[Pasted image 20250917112202.png]]

# 3. 安装 Element Plus

## 3.1 安装

```shell
pnpm install element-plus --save
# pnpm install element-plus
pnpm install @element-plus/icons-vue
```

![[Pasted image 20250917113305.png]]

![[Pasted image 20250917113323.png]]

## 3.2 在 `main.ts` 中引入 Element Plus

```ts
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

app.use(ElementPlus)

//全局注册图标组件
for (const [key, component] of 
Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

![[Pasted image 20250917113923.png]]

## 3.3 安装相关插件

![[Pasted image 20250917113716.png]]

## 3.4 测试

在 `HelloWorld.vue` 文件内修改代码为

```vue
<script setup lang="ts">

</script>

<template>
  <div>Hello World</div>
  <el-button type="primary">Primary Button</el-button>
</template>

<style lang="scss" scoped>

</style>
```

![[Pasted image 20250917114129.png]]

成功展示按钮，引入成功。

# 4. 初识 Pinia 与安装

官网：[Pinia | The intuitive store for Vue.js](https://pinia.vuejs.org/)

## 4.1 安装依赖

```shell
pnpm add pinia
```

![[Pasted image 20250917120902.png]]

## 4.2 在 `main.ts` 引入 Pinia

```ts
//引入Pinia构造函数
import { createPinia } from 'pinia'

// 实例化 Pinia
const pinia = createPinia()

app.use(pinia)
```

![[Pasted image 20250917143702.png]]

## 4.3 使用 Pinia

### 4.3.1 定义仓库

在 src 下新建 store 文件夹，然后在 store 文件夹建立 `index.ts` 文件。

因为方才下载的 `pinia` 的版本是 3.x，故使用了 Getters 和 Actions 的新写法。若下载的 `pinia` 的版本是 2.x，则采用选项式或者组合式 API 的写法。

```ts
import { defineStore } from 'pinia'
import { ref, reactive, computed } from 'vue'

export const GlobalStore = defineStore('GlobalState', () => {
  // State
  const token = ref('')
  const userInfo = reactive({
    username: '',
    id: -1,
    token: '',
  })

  // Getters (使用 computed)
  const isLoggedIn = computed(() => !!token.value)
  const userId = computed(() => userInfo.id)
  const userName = computed(() => userInfo.username)

  // Actions (函数)
  function setToken(newToken: string) {
    token.value = newToken
    userInfo.token = newToken
  }

  function setUserInfo(info: Partial<typeof userInfo>) {
    Object.assign(userInfo, info)
  }

  function clearUser() {
    token.value = ''
    Object.assign(userInfo, {
      username: '',
      id: -1,
      token: '',
    })
  }

  return {
    // State
    token,
    userInfo,
    
    // Getters
    isLoggedIn,
    userId,
    userName,
    
    // Actions
    setToken,
    setUserInfo,
    clearUser,
  }
})
```

![[Pasted image 20250917145851.png]]

### 4.3.2 修改 `HelloWorld.vue` 并进行测试

```vue
<script setup lang="ts">
import { GlobalStore } from '@/store'

const store = GlobalStore()

function showUserInfoId() {
  console.log(store.userInfo.id)
}
</script>

<template>
  <div>Hello World</div>
  <el-button
    @click="showUserInfoId"
    type="primary"
  >
    Primary Button
  </el-button>
</template>
```

![[Pasted image 20250917153408.png]]

![[Pasted image 20250917150427.png]]

# 5. 主界面布局

## 5.1 安装sass

```shell
pnpm add --save-dev sass
```

![[Pasted image 20250918085412.png]]

## 5.2 找到 `index.html` 添加如下 style 样式

```html
<style>
  html, body, #app {
    padding: 0px;
    margin: 0px;
    height: 100%;
    width: 100%;
    box-sizing: border-box;
  }
</style>
```

![[Pasted image 20250922132616.png]]

## 5.3 在 src 目录下新建 layout 目录，并新建 `index.vue` 主页面组件

从 Element Plus 官网（ https://element-plus.sxtxhy.com/zh-CN/ ）复制页面布局。

![[Pasted image 20250918093904.png]]

```vue
<template>
  <el-container class="layout">
    <el-aside width="200px" class="aside">Aside</el-aside>
    <el-container>
      <el-header class="header">Header</el-header>
      <el-main class="main">Main</el-main>
    </el-container>
  </el-container>
</template>

<style lang="scss">
.layout {
  height: 100%;
  .aside {
    background-color: rgb(162, 139, 182);
  }
  .header {
    background-color: rgb(225, 170, 132);
  }
  .main {
    background-color: rgb(153, 198, 198);
  }
}
</style>
```

![[Pasted image 20250922132743.png]]

## 5.4 在 router 中引入主页面组件

在 router 目录下的 `index.ts` 中修改路径。

![[Pasted image 20250922132952.png]]

启动项目，查看效果。

![[Pasted image 20250922133019.png]]

# 6 左侧导航菜单制作

对应的 Element Plus 组件库中的菜单：[Menu 菜单 | Element Plus](https://element-plus.org/zh-CN/component/menu)

## 6.1 创建 Header 组件

在 layout 文件夹下新建 Header 目录，然后新建 `index.vue` 组件

```vue
<template>
  <div>Layout-Header</div>
</template>
```

![[Pasted image 20251008162115.png]]

## 6.2 创建 Menu 目录

在 `layout` 文件夹下新建 Menu 目录，然后新建 `index.vue` 组件和 components 文件夹中的 `left-menu.vue` 组件。

目录结构如下。

![[Pasted image 20251008162605.png]]

### 6.2.1 `layout\Menu\index.vue` 文件

```vue
<script setup lang="ts">
import { ref, reactive } from "vue"
import LeftMenu from "./components/left-menu.vue"

const isCollapse = ref(false);
const menuList = reactive([
  {
    path: "/system",
    component: "Layout",
    name: "system",
    meta: {
      title: "系统管理",
      icon: "Setting",
      roles: ["sys:manage"],
    },
    children: [
      {
        path: "/userList",
        component: "/system/User/UserList",
        name: "userList",
        meta: {
          title: "员工管理",
          icon: "UserFilled",
          roles: ["sys:user"],
        },
      },
      {
        path: "/roleList",
        component: "/system/Role/RoleList",
        name: "roleList",
        meta: {
          title: "角色管理",
          icon: "Wallet",
          roles: ["sys:role"],
        },
      },
      {
        path: "/menuList",
        component: "/system/Menu/MenuList",
        name: "menuList",
        meta: {
          title: "菜单管理",
          icon: "Menu",
          roles: ["sys:menu"],
        },
      },
    ],
  },
  {
    path: "/memberRoot",
    component: "Layout",
    name: "memberRoot",
    meta: {
      title: "会员管理",
      icon: "Setting",
      roles: ["sys:memberRoot"],
    },
    children: [
      {
        path: "/cardType",
        component: "/member/CardType",
        name: "cardType",
        meta: {
          title: "会员卡类型",
          icon: "UserFilled",
          roles: ["sys:cardType"],
        },
      },
      {
        path: "/memberList",
        component: "/member/MemberList",
        name: "memberList",
        meta: {
          title: "会员管理",
          icon: "Wallet",
          roles: ["sys:memberList"],
        },
      },
      {
        path: "/myFee",
        component: "/system/FeeList",
        name: "myFee",
        meta: {
          title: "我的充值",
          icon: "Menu",
          roles: ["sys:myFee"],
        },
      },
    ],
  },
])
</script>

<template>
  <el-menu
    default-active="2"
    class="el-menu-vertical-demo"
    :collapse="isCollapse"
    unique-opened
    background-color="#304156"
  >
    <left-menu :menuList="menuList" />
  </el-menu>
</template>

<style lang="scss" scoped>
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 230px;
  min-height: 400px;
}
.el-menu {
  height: 100%;
  border-right: none;
}
:deep(.el-sub-menu .el-sub-menu__title){
  color: #f4f4f5 !important;
}
:deep(.el-menu .el-menu-item){
  color: #bfcbd9;
}
/* 菜单点中文字的颜色 */
:deep(.el-menu-item.is-active){
  color: #409eff !important;
}
/* 当前打开菜单的所有子菜单颜色 */
:deep(.is-opened .el-menu-item){
  background-color: #1f2d3d !important;
}
/* 鼠标移动菜单的颜色 */
:deep(.el-menu-item:hover){
  background-color: #001528 !important;
}
</style>
```

### 6.2.2 `layout\Menu\components\left-menu.vue` 文件

```vue
<script setup lang="ts">
defineProps(["menuList"]);
</script>

<template>
  <template
    v-for="menu in menuList"
    :key="menu.path"
  >
    <el-sub-menu
      v-if="menu.children && menu.children.length > 0" 
      :index="menu.path"
    >
        <template #title>
          <el-icon>
              <component :is="menu.meta.icon"></component>
          </el-icon>
          <span>{{ menu.meta.title }}</span>
        </template>
        <left-menu :menuList="menu.children"></left-menu>
    </el-sub-menu>
    <el-menu-item
      v-else
      style="color: #f4f4f5" 
      :index="menu.path"
    >
      <el-icon>
        <component :is="menu.meta.icon"></component>
      </el-icon>
      <template #title>{{ menu.meta.title }}</template>
    </el-menu-item>
  </template>
</template>
```

## 6.3 修改 `layout/index.vue` 组件

```vue
<script lang="ts" setup>
import CustomHeader from './Header/index.vue'
import CustomMenu from './Menu/index.vue'
</script>

<template>
  <div class="full-screen">
    <el-container class="layout">
      <el-aside width="auto" class="aside">
        <custom-menu />
      </el-aside>
      <el-container>
        <el-header class="header">
          <custom-header />
        </el-header>
        <el-main class="main">Main</el-main>
      </el-container>
    </el-container>
  </div>
</template>

<style lang="scss">
.full-screen {
  height: 100vh; /* 视口高度 */
  width: 100vw; /* 视口宽度 */
}
.layout {
  height: 100%;
  .aside {
  	height: 100%;
    background-color: rgb(162, 139, 182);
  }
  .header {
    background-color: rgb(225, 170, 132);
  }
  .main {
    background-color: rgb(153, 198, 198);
  }
}
</style>
```

## 6.4 成果展示

![[Pasted image 20251008162455.png]]

# 7.菜单logo制作

## 7.1 项目 `assets` 里面加入logo

![[Pasted image 20251008163628.png]]

## 7.2 修改 `layout\Menu\index.vue` 文件

添加 logo 和项目名称。

```vue
<script setup lang="ts">
import { ref, reactive } from "vue"
import LeftMenu from "./components/left-menu.vue"
import MenuLogo from '@/assets/logo.jpg'

const isCollapse = ref(false);
const menuList = reactive([
  {
    path: "/system",
    component: "Layout",
    name: "system",
    meta: {
      title: "系统管理",
      icon: "Setting",
      roles: ["sys:manage"],
    },
    children: [
      {
        path: "/userList",
        component: "/system/User/UserList",
        name: "userList",
        meta: {
          title: "员工管理",
          icon: "UserFilled",
          roles: ["sys:user"],
        },
      },
      {
        path: "/roleList",
        component: "/system/Role/RoleList",
        name: "roleList",
        meta: {
          title: "角色管理",
          icon: "Wallet",
          roles: ["sys:role"],
        },
      },
      {
        path: "/menuList",
        component: "/system/Menu/MenuList",
        name: "menuList",
        meta: {
          title: "菜单管理",
          icon: "Menu",
          roles: ["sys:menu"],
        },
      },
    ],
  },
  {
    path: "/memberRoot",
    component: "Layout",
    name: "memberRoot",
    meta: {
      title: "会员管理",
      icon: "Setting",
      roles: ["sys:memberRoot"],
    },
    children: [
      {
        path: "/cardType",
        component: "/member/CardType",
        name: "cardType",
        meta: {
          title: "会员卡类型",
          icon: "UserFilled",
          roles: ["sys:cardType"],
        },
      },
      {
        path: "/memberList",
        component: "/member/MemberList",
        name: "memberList",
        meta: {
          title: "会员管理",
          icon: "Wallet",
          roles: ["sys:memberList"],
        },
      },
      {
        path: "/myFee",
        component: "/system/FeeList",
        name: "myFee",
        meta: {
          title: "我的充值",
          icon: "Menu",
          roles: ["sys:myFee"],
        },
      },
    ],
  },
])
</script>

<template>
  <el-menu
    default-active="2"
    class="el-menu-vertical-demo"
    :collapse="isCollapse"
    unique-opened
    background-color="#304156"
  >
    <div class="logo">
      <img :src="MenuLogo" alt="logo" />
      <div class="logo-title">
        <span>图书借阅</span>
        <span>管理系统</span>
      </div>
    </div>
    <left-menu :menuList="menuList" />
  </el-menu>
</template>

<style lang="scss" scoped>
.logo {
  display: flex;
  width: 100%;
  height: 80px;
  line-height: 60px;
  text-align: center;
  cursor: pointer;
  align-items: center;
  justify-content: center;

  img {
    width: 36px;
    height: 36px;
    margin-right: 12px;
    margin-left: 12px;
  }

  .logo-title {
    color: #fff;
    font-weight: 800;
    font-size: 22px;
    line-height: normal;
    display: flex;
    flex-direction: column;
  }
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
  min-height: 400px;
}
.el-menu {
  height: 100%;
  border-right: none;
}
:deep(.el-sub-menu .el-sub-menu__title){
  color: #f4f4f5 !important;
}
:deep(.el-menu .el-menu-item){
  color: #bfcbd9;
}
/* 菜单点中文字的颜色 */
:deep(.el-menu-item.is-active){
  color: #409eff !important;
}
/* 当前打开菜单的所有子菜单颜色 */
:deep(.is-opened .el-menu-item){
  background-color: #1f2d3d !important;
}
/* 鼠标移动菜单的颜色 */
:deep(.el-menu-item:hover){
  background-color: #001528 !important;
}
</style>
```

![[Pasted image 20251008223801.png]]

![[Pasted image 20251008223827.png]]

## 7.3 成果展示

![[Pasted image 20251008223852.png]]

# 8.路由配置与页面创建

要求：点击左侧菜单，能够在内容展示区展示对应页面。

## 8.1 修改路由

`router/index.ts` 路由修改为如下。

```ts
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'

const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    redirect: '/home',
  },
  {
    path: '/home',
    component: () => import('@/layout/index.vue'),
    redirect: '/home',
    children: [
      {
        path: '/home',
        component: () => import('@/views/home/index.vue'),
        name: 'home',
        meta: {
          title: '首页',
          icon: '#iconhome',
        }
      },
    ],
  },
  {
    path: "/system",
    component: () => import('@/layout/index.vue'),
    name: "system",
    meta: {
      title: "系统管理",
      icon: "el-icon-menu",
      roles: ["sys:manage"]
    },
    children: [
      {
        path: "/userList",
        component: () => import('@/views/system/UserList.vue'),
        name: "userList",
        meta: {
          title: "员工管理",
          icon: "el-icon-s-custom",
          roles: ["sys:user"]
        },
      },
      {
        path: "/roleList",
        component: () => import('@/views/system/RoleList.vue'),
        name: "roleList",
        meta: {
          title: "角色管理",
          icon: "el-icon-s-tools",
          roles: ["sys:role"]
        },
      },
      {
        path: "/menuList",
        component: () => import('@/views/system/MenuList.vue'),
        name: "menuList",
        meta: {
          title: "权限管理",
          icon: "el-icon-document",
          roles: ["sys:menu"]
        },
      },
    ]
  },
  {
    path: "/memberRoot",
    component: () => import('@/layout/index.vue'),
    name: "memberRoot",
    meta: {
      title: "会员管理",
      icon: "Setting",
      roles: ["sys:memberRoot"],
    },
    children: [
      {
        path: "/cardType",
        component: () => import('@/views/member/CardType.vue'),
        name: "cardType",
        meta: {
          title: "会员卡类型",
          icon: "UserFilled",
          roles: ["sys:cardType"],
        },
      },
      {
        path: "/memberList",
        component: () => import('@/views/member/MemberList.vue'),
        name: "memberList",
        meta: {
          title: "会员管理",
          icon: "Wallet",
          roles: ["sys:memberList"],
        },
      },
      {
        path: "/myFee",
        component: () => import('@/views/member/MyFee.vue'),
        name: "myFee",
        meta: {
        title: "我的充值",
        icon: "Menu",
        roles: ["sys:myFee"],
        },
      },
    ],
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes,
  strict: false,
  // 切换页面，滚动到最顶部
  scrollBehavior: () => ({ left: 0, top: 0 }),
})

export default router
```

## 8.2 创建路由对应的页面

新建 `views` 文件夹，然后创建对应 vue 文件。

![[Pasted image 20251008231651.png]]

## 8.3 在 `layout\Menu\index.vue` 组件的 `<el-menu>` 添加 router 属性

router: 是否启用 vue-router 模式。启用该模式会在激活导航时以 index 作为 path 进行路由跳转。

![[Pasted image 20251008231848.png]]

## 8.4 在 `layout\index.vue` 的添加路由

![[Pasted image 20251008232002.png]]

```vue
<script lang="ts" setup>
import CustomHeader from './Header/index.vue'
import CustomMenu from './Menu/index.vue'
</script>

<template>
  <div class="full-screen">
    <el-container class="layout">
      <el-aside width="auto" class="aside">
        <custom-menu />
      </el-aside>
      <el-container>
        <el-header class="header">
          <custom-header />
        </el-header>
        <el-main class="main">
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<style lang="scss">
.full-screen {
  height: 100vh; /* 视口高度 */
  width: 100vw; /* 视口宽度 */
}
.layout {
  height: 100%;
  .aside {
    height: 100%;
    width: 213px;
    background-color: rgb(162, 139, 182);
  }
  .header {
    background-color: rgb(225, 170, 132);
  }
  .main {
    background-color: rgb(153, 198, 198);
  }
}
</style>
```

## 8.5 在 `layout\Menu\index.vue` 组件设置当前激活的菜单

```vue
<script setup lang="ts">
import { ref, reactive, computed } from "vue"
import { useRoute } from "vue-router"
import LeftMenu from "./components/left-menu.vue"
import MenuLogo from '@/assets/logo.jpg'

const route = useRoute()
const isCollapse = ref(false)
const menuList = reactive([
  {
    path: "/home",
    component: "Layout",
    name: "home",
    meta: {
      title: "首页",
      icon: "House",
      roles: ["sys:home"],
    },
  },
  {
    path: "/system",
    component: "Layout",
    name: "system",
    meta: {
      title: "系统管理",
      icon: "Setting",
      roles: ["sys:manage"],
    },
    children: [
      {
        path: "/userList",
        component: "/system/User/UserList",
        name: "userList",
        meta: {
          title: "员工管理",
          icon: "UserFilled",
          roles: ["sys:user"],
        },
      },
      {
        path: "/roleList",
        component: "/system/Role/RoleList",
        name: "roleList",
        meta: {
          title: "角色管理",
          icon: "Wallet",
          roles: ["sys:role"],
        },
      },
      {
        path: "/menuList",
        component: "/system/Menu/MenuList",
        name: "menuList",
        meta: {
          title: "菜单管理",
          icon: "Menu",
          roles: ["sys:menu"],
        },
      },
    ],
  },
  {
    path: "/memberRoot",
    component: "Layout",
    name: "memberRoot",
    meta: {
      title: "会员管理",
      icon: "Setting",
      roles: ["sys:memberRoot"],
    },
    children: [
      {
        path: "/cardType",
        component: "/member/CardType",
        name: "cardType",
        meta: {
          title: "会员卡类型",
          icon: "UserFilled",
          roles: ["sys:cardType"],
        },
      },
      {
        path: "/memberList",
        component: "/member/MemberList",
        name: "memberList",
        meta: {
          title: "会员管理",
          icon: "Wallet",
          roles: ["sys:memberList"],
        },
      },
      {
        path: "/myFee",
        component: "/system/FeeList",
        name: "myFee",
        meta: {
          title: "我的充值",
          icon: "Menu",
          roles: ["sys:myFee"],
        },
      },
    ],
  },
])

//获取激活的菜单
const activeIndex = computed(()=>{
  const {path} = route;
  return path;
})
</script>

<template>
  <el-menu
    default-active="activeIndex"
    class="el-menu-vertical-demo"
    :collapse="isCollapse"
    unique-opened
    background-color="#304156"
    router
  >
    <div class="logo">
      <img :src="MenuLogo" alt="logo" />
      <div class="logo-title">
        <span>图书借阅</span>
        <span>管理系统</span>
      </div>
    </div>
    <left-menu :menuList="menuList" />
  </el-menu>
</template>

<style lang="scss" scoped>
.logo {
  display: flex;
  width: 100%;
  height: 80px;
  line-height: 60px;
  text-align: center;
  cursor: pointer;
  align-items: center;
  justify-content: center;

  img {
    width: 36px;
    height: 36px;
    margin-right: 12px;
    margin-left: 12px;
  }

  .logo-title {
    color: #fff;
    font-weight: 800;
    font-size: 22px;
    line-height: normal;
    display: flex;
    flex-direction: column;
  }
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
  min-height: 400px;
}
.el-menu {
  height: 100%;
  border-right: none;
}
:deep(.el-sub-menu .el-sub-menu__title){
  color: #f4f4f5 !important;
}
:deep(.el-menu .el-menu-item){
  color: #bfcbd9;
}
/* 菜单点中文字的颜色 */
:deep(.el-menu-item.is-active){
  color: #409eff !important;
}
/* 当前打开菜单的所有子菜单颜色 */
:deep(.is-opened .el-menu-item){
  background-color: #1f2d3d !important;
}
/* 鼠标移动菜单的颜色 */
:deep(.el-menu-item:hover){
  background-color: #001528 !important;
}
</style>
```

![[Pasted image 20251008232233.png]]

# 9.菜单收缩

## 9.1 `store` 文件夹下新建 `modules` 文件夹，然后新建 `menu.ts`

在 `store` 文件夹下新建 `modules` 文件夹，然后新建 `menu.ts` 文件。

```ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import type { MenuOptions } from '../interface'

export const useMenuStore = defineStore('MenuState', () => {
  // state
  const isCollapse = ref(false)
  const menuList = ref<MenuOptions[]>([])
  const activeMenu = ref('learning-square')
  const closeMenuList = ref<string[]>([])

  // getters
  const getActiveMenu = computed(() => activeMenu.value)
  const getCloseMenuList = computed(() => closeMenuList.value)
  const getMenuList = computed(() => menuList.value)

  // actions
  function setCollapse() {
    isCollapse.value = !isCollapse.value
  }
  
  function setMenuList(list: MenuOptions[]) {
    menuList.value = list
  }
  
  function setActiveMenu(menu: string) {
    activeMenu.value = menu
  }
  
  function setCloseMenuList(list: string[]) {
    closeMenuList.value = list
  }

  return {
    // state
    isCollapse,
    menuList,
    activeMenu,
    closeMenuList,
    // getters
    getActiveMenu,
    getCloseMenuList,
    getMenuList,
    // actions
    setCollapse,
    setMenuList,
    setActiveMenu,
    setCloseMenuList,
  }
}, {
  // persist: {
  //   paths: ['activeMenu'],
  // },
  persist: true,
})
```

在 `store` 文件夹下新建 `interface` 文件夹，然后新建 `index.ts` 文件。

```ts
interface Meta {
  permission?: string
  icon?: string
  title: string
}

interface Child {
  permission: string
  path: string
  name: string
  component: string
  icon: string
  meta: {
    title: string
  }
}

export interface MenuOptions {
  path?: string
  meta: Meta
  name: string
  top?: number
  icon: string
  children?: Child[]
}

export interface ThemeConfigProp {
  primary: string
  isGrey: boolean
  isWeak: boolean
}

/* GlobalState */
export interface GlobalState {
  token: string
  userInfo: any
  language: string
  themeConfig: ThemeConfigProp
}

/* MenuState */
export interface MenuState {
  isCollapse: boolean
  menuList: MenuOptions[]
  activeMenu: string
  closeMenuList: string[]
}

/* TabsState */
export interface TabsState {
  tabsMenuValue: string
  tabsMenuList: MenuOptions[]
}

/* AuthState */
export interface AuthState {
  authButtons: {
    [propName: string]: any
  }
  authRouter: string[]
}
```

目录结构如下。

![[Pasted image 20251009205621.png]]

## 9.2 引入 `pinia-plugin-persistedstate` 插件

若 9.1 报错，则下载 `pinia-plugin-persistedstate` 插件。

```shell
pnpm add pinia-plugin-persistedstate
```

并修改 `main.ts` 文件。

```ts
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router/index'
// 引入element-plus
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
//引入Pinia构造函数
import { createPinia } from 'pinia'
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate' // 插件

const pinia = createPinia()
pinia.use(piniaPluginPersistedstate)

const app = createApp(App)
app.use(ElementPlus)
app.use(router)
app.use(pinia)
app.mount('#app')

//全局注册图标组件
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}
```

![[Pasted image 20251009205857.png]]

## 9.3 在 `layout/Header` 下新建 `collapse.vue` 组件

```vue
<script setup lang="ts">
import { computed } from "vue";
import { Fold, Expand } from "@element-plus/icons-vue";
import { useMenuStore } from "@/store/modules/menu";

// 获取store
const store = useMenuStore();
// 获取collapse状态
const status = computed(() => {
  return !store.isCollapse; // 直接访问 isCollapse 状态
});
// 改变collapse状态
const setCollapse = () => {
  store.setCollapse(); // 调用 store 中的 setCollapse 方法
}
</script>

<template>
  <el-icon class="icons" @click="setCollapse">
    <component :is="status ? Fold : Expand" />
  </el-icon>
</template>

<style lang="scss" scoped>
.icons {
  color: #fff;
  font-size: 24px;
}
</style>
```

目录结构及代码如下。

![[Pasted image 20251009210033.png]]

## 9.4 修改 `Header` 文件夹中的 `index.ts` 组件

```vue
<script setup lang="ts">
import Collapse from './components/collapse.vue';
</script>

<template>
  <Collapse />
</template>
```

![[Pasted image 20251009210156.png]]

## 9.5  修改 `layout\index.vue` 的 `header` 样式

删除原有 `width`，添加布局。

```vue
<script lang="ts" setup>
import CustomHeader from './Header/index.vue'
import CustomMenu from './Menu/index.vue'
</script>

<template>
  <div class="full-screen">
    <el-container class="layout">
      <el-aside width="auto" class="aside">
        <custom-menu />
      </el-aside>
      <el-container>
        <el-header class="header">
          <custom-header />
        </el-header>
        <el-main class="main">
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<style lang="scss">
.full-screen {
  height: 100vh; /* 视口高度 */
  width: 100vw; /* 视口宽度 */
}
.layout {
  height: 100%;
  .aside {
    height: 100%;
    background-color: rgb(162, 139, 182);
  }
  .header {
    background-color: rgb(225, 170, 132);
    display: flex;
    align-items: center;
  }
  .main {
    background-color: rgb(153, 198, 198);
  }
}
</style>
```

![[Pasted image 20251009210325.png]]

## 9.6 修改 `layout\Menu\index.vue` 组件

```vue
<script setup lang="ts">
import { reactive, computed } from "vue"
import { useRoute } from "vue-router"
import { useMenuStore } from "@/store/modules/menu"
import LeftMenu from "./components/left-menu.vue"
import MenuLogo from '@/assets/logo.jpg'

const route = useRoute()
const colstore = useMenuStore();
const isCollapse = computed(() => {
  return !colstore.isCollapse;
});
const menuList = reactive([
  {
    path: "/home",
    component: "Layout",
    name: "home",
    meta: {
      title: "首页",
      icon: "House",
      roles: ["sys:home"],
    },
  },
  {
    path: "/system",
    component: "Layout",
    name: "system",
    meta: {
      title: "系统管理",
      icon: "Setting",
      roles: ["sys:manage"],
    },
    children: [
      {
        path: "/userList",
        component: "/system/User/UserList",
        name: "userList",
        meta: {
          title: "员工管理",
          icon: "UserFilled",
          roles: ["sys:user"],
        },
      },
      {
        path: "/roleList",
        component: "/system/Role/RoleList",
        name: "roleList",
        meta: {
          title: "角色管理",
          icon: "Wallet",
          roles: ["sys:role"],
        },
      },
      {
        path: "/menuList",
        component: "/system/Menu/MenuList",
        name: "menuList",
        meta: {
          title: "菜单管理",
          icon: "Menu",
          roles: ["sys:menu"],
        },
      },
    ],
  },
  {
    path: "/memberRoot",
    component: "Layout",
    name: "memberRoot",
    meta: {
      title: "会员管理",
      icon: "Setting",
      roles: ["sys:memberRoot"],
    },
    children: [
      {
        path: "/cardType",
        component: "/member/CardType",
        name: "cardType",
        meta: {
          title: "会员卡类型",
          icon: "UserFilled",
          roles: ["sys:cardType"],
        },
      },
      {
        path: "/memberList",
        component: "/member/MemberList",
        name: "memberList",
        meta: {
          title: "会员管理",
          icon: "Wallet",
          roles: ["sys:memberList"],
        },
      },
      {
        path: "/myFee",
        component: "/system/FeeList",
        name: "myFee",
        meta: {
          title: "我的充值",
          icon: "Menu",
          roles: ["sys:myFee"],
        },
      },
    ],
  },
])

//获取激活的菜单
const activeIndex = computed(()=>{
  const {path} = route;
  return path;
})
</script>

<template>
  <el-menu
    default-active="activeIndex"
    class="el-menu-vertical-demo"
    :collapse="isCollapse"
    unique-opened
    background-color="#304156"
    router
  >
    <div class="logo">
      <img v-if="!isCollapse" :src="MenuLogo" alt="logo" />
      <div v-if="!isCollapse" class="logo-title">
        <span>图书借阅</span>
        <span>管理系统</span>
      </div>
      <div v-else class="logo-title">
        <span>系</span>
        <span>统</span>
      </div>
    </div>
    <left-menu :menuList="menuList" />
  </el-menu>
</template>

<style lang="scss" scoped>
.logo {
  display: flex;
  width: 100%;
  height: 80px;
  line-height: 60px;
  text-align: center;
  cursor: pointer;
  align-items: center;
  justify-content: center;

  img {
    width: 36px;
    height: 36px;
    margin-right: 12px;
    margin-left: 12px;
  }

  .logo-title {
    color: #fff;
    font-weight: 800;
    font-size: 22px;
    line-height: normal;
    display: flex;
    flex-direction: column;
  }
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 213px;
  min-height: 400px;
}
.el-menu {
  height: 100%;
  border-right: none;
}
:deep(.el-sub-menu .el-sub-menu__title){
  color: #f4f4f5 !important;
}
:deep(.el-menu .el-menu-item){
  color: #bfcbd9;
}
/* 菜单点中文字的颜色 */
:deep(.el-menu-item.is-active){
  color: #409eff !important;
}
/* 当前打开菜单的所有子菜单颜色 */
:deep(.is-opened .el-menu-item){
  background-color: #1f2d3d !important;
}
/* 鼠标移动菜单的颜色 */
:deep(.el-menu-item:hover){
  background-color: #001528 !important;
}
</style>
```

添加 `:collapse` 属性。

![[Pasted image 20251009210512.png]]

修改 logo 及标题的缩放属性。

![[Pasted image 20251009210600.png]]

添加目录展开时的宽度。

![[Pasted image 20251009210646.png]]

## 9.7 成果展示

目录展开。

![[Pasted image 20251009210741.png]]

目录收拢。

![[Pasted image 20251009210759.png]]

# 10.面包屑导航制作

参考官方文档：[[[Breadcrumb 面包屑 | Element Plus](https://element-plus.org/zh-CN/component/breadcrumb)]]

## 10.1 新建 `layout\Header\components\bread-crumb.vue` 文件

