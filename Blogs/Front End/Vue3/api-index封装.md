---
tags:
  - VUE3
---
```ts
/*
 * @Author: yujingbo
 * @Date: 2022-11
 * @LastEditors: zengchao
 * @LastEditTime: 2024-11
 * @Description:
 */
import type { AxiosError, AxiosInstance, AxiosRequestConfig, AxiosRequestHeaders, AxiosResponse } from 'axios'
import axios from 'axios'
import { ElMessage } from 'element-plus'
import { AxiosCanceler } from './helper/axiosCancel'
import { checkStatus } from './helper/checkStatus'
import { showFullScreenLoading, tryHideFullScreenLoading } from '@/config/serviceLoading'
import type { ResultData } from '@/api/interface'
import { GlobalStore } from '@/store'
import router from '@/routers'

/**
 * pinia 错误使用说明
 * https://github.com/vuejs/pinia/discussions/971
 * https://github.com/vuejs/pinia/discussions/664#discussioncomment-1329898
 * https://pinia.vuejs.org/core-concepts/outside-component-usage.html#single-page-applications
 */
// const globalStore = GlobalStore();

export enum ResultEnum {
  SUCCESS = 0,
  OVERDUEMIN = 10000,
  OVERDUEMAX = 19999,
  ERRORWITHOUTPROMPT = 90001,
}

const axiosCanceler = new AxiosCanceler()

const config = {
  // 默认地址请求地址，可在 .env 开头文件中修改
  baseURL: import.meta.env.VITE_API_URL as string,
  // 设置超时时间
  timeout: 60000,
  // 跨域时候允许携带凭证
  withCredentials: true,
  headers: { noLoading: true },
}

class RequestHttp {
  service: AxiosInstance
  public constructor(config: AxiosRequestConfig) {
    // 实例化axios
    this.service = axios.create(config)

    /**
     * @description 请求拦截器
     * 客户端发送请求 -> [请求拦截器] -> 服务器
     * token校验(JWT) : 接受服务器返回的token,存储到vuex/本地储存当中
     */
    this.service.interceptors.request.use(
      (config: AxiosRequestConfig) => {
        const globalStore = GlobalStore()
        // * 将当前请求添加到 pending 中 临时注释
        // axiosCanceler.addPending(config)
        // * 如果当前请求不需要显示 loading,在api服务中通过指定的第三个参数: { headers: { noLoading: true } }来控制不显示loading，参见loginApi
        config.headers!.noLoading || showFullScreenLoading()
        if (!globalStore.token)
          return { ...config, headers: config.headers! as AxiosRequestHeaders }

        const organizationId = globalStore.userInfo.last_selected_group_id
        if (organizationId !== -1 && config.url)
          config.url += `${!config.url.includes('?') ? '?' : '&'}current_group=${organizationId}`

        return { ...config, headers: { ...config.headers, Authorization: `Bearer ${globalStore.token}` } as AxiosRequestHeaders }
      },
      (error: AxiosError) => {
        return Promise.reject(error)
      },
    )

    /**
     * @description 响应拦截器
     *  服务器换返回信息 -> [拦截统一处理] -> 客户端JS获取到信息
     */
    this.service.interceptors.response.use(
      (response: AxiosResponse) => {
        const { data, config } = response
        const globalStore = GlobalStore()
        // * 在请求结束后，移除本次请求
        axiosCanceler.removePending(config)
        tryHideFullScreenLoading()
        // * 登陆失效 认证异常
        if (data.code >= ResultEnum.OVERDUEMIN && data.code <= ResultEnum.OVERDUEMAX) {
          ElMessage.error(data.message)
          globalStore.token = ''
          void router.replace({
            path: '/login',
          })
          return Promise.reject(data)
        }

        // * 全局错误信息拦截（防止下载文件得时候返回数据流，直接报错）
        if (data.code > ResultEnum.SUCCESS) {
          if (data.code !== ResultEnum.ERRORWITHOUTPROMPT)
            ElMessage.error(data.message)

          return Promise.reject(data)
        }

        // * 成功请求
        return data
      },
      async (error: AxiosError) => {
        const { response } = error
        tryHideFullScreenLoading()
        // 根据响应的错误状态码，做不同的处理
        if (response)
          return checkStatus(response.status)
        return Promise.reject(error)
      },
    )
  }

  // * 常用请求方法封装
  get<T = any>(url: string, params?: object, _object = {}): Promise<ResultData<T>> {
    return this.service.get(url, { params, ..._object })
  }

  post<T = any>(url: string, params?: object, _object = {}): Promise<ResultData<T>> {
    return this.service.post(url, params, _object)
  }

  put<T = any>(url: string, params?: object, _object = {}): Promise<ResultData<T>> {
    return this.service.put(url, params, _object)
  }

  delete<T = any>(url: string, params?: any, _object = {}): Promise<ResultData<T>> {
    return this.service.delete(url, { params, ..._object })
  }

  patch<T = any>(url: string, params?: object, _object = {}): Promise<ResultData<T>> {
    return this.service.patch(url, params, _object)
  }
}

export default new RequestHttp(config)
```

