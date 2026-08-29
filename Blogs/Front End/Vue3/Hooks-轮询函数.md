---
tags:
  - 前端
  - VUE3
  - hooks
---
```ts
/*
 * @Author: Mr-Nobody-li
 * @Date: 2022-08
 * @LastEditors: yujingbo
 * @LastEditTime: 2023-12
 * @Description: 轮询
 */
import { onActivated, onDeactivated, onUnmounted } from 'vue'

type Callback = () => void

export default (callback: Callback, timeInterval = 60000) => {
  let timer: ReturnType<typeof setTimeout> | undefined
  const create = () => {
    timer = setInterval(() => {
      callback()
    }, timeInterval)
  }
  create()
  onUnmounted(() => {
    clearInterval(timer)
  })
  onActivated(() => {
    callback() // 激活时执行一次
    if (!timer)
      create()
  })
  onDeactivated(() => {
    clearInterval(timer)
    timer = undefined
  })
  return {
    timer,
  }
}

```