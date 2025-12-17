---
title: 优化axios封装之配置使用AbortController取消重复请求
date: 2024-09-24 17:21:14
tags:
---

本文主要记录如何使用 axios 的 CancelToken 机制解决数据请求过多导致的数据错乱问题，通过配置参数中断重复请求并管理缓存，确保请求的高效性和准确性。  
当用户重复调用一个接口时，会导致数据错乱，解决方法是取消上一次重复的请求，通过<code>axios</code>的<code>AbortController</code>去中断之前的请求。  
**PS：AbortController 仅支持 Axios0.22.0 及以上的版本，本人使用的是 0.22.0 的版本，请确保使用版本不低于这个版本**

**在封装的 axios 文件中增加逻辑**

# 定义变量

```js
// isCancel-取消标识 用于判断请求是不是被AbortController取消的
const { isCancel } = axios;
const cacheRequest = {};
```

# 定义删除缓存队列中请求的函数

```js
// 删除缓存队列中的请求
function removeCacheRequest(reqKey) {
  if (cacheRequest[reqKey]) {
    // 通过AbortController实例上的abort来进行请求的取消
    cacheRequest[reqKey].abort();
    delete cacheRequest[reqKey];
  }
}
```

# 在请求 request 拦截器里添加标识 将需要清除的请求增加到缓存队列里

```js
/**
 * 请求前的拦截
 */
instance.interceptors.request.use((config) => {
  // removeCache - 是config里配置的是否清除相同请求的标识，不传则默认是不需要清除
  // 此处根据实际需求来 如果需要全部清除相同的请求功能 则默认为true 或者增加白名单
  const { url, method, removeCache } = config;
  if (removeCache) {
    // 请求地址和请求方式组成唯一标识，将这个标识作为取消函数的key，保存到请求队列中
    const reqKey = `${url}&${method}`;
    // 如果config传了需要清除重复请求的removeCache，则如果存在重复请求，删除之前的请求
    removeCacheRequest(reqKey);
    // 将请求加入请求队列，通过AbortController来进行手动取消
    const controller = new AbortController();
    config.signal = controller.signal;
    cacheRequest[reqKey] = controller;
  }
});
```

# 在响应 response 拦截器里请求成功后在队列里移除，请求失败 error 里判断，使得 CancelToken 取消的请求不做任何处理

```js
instance.interceptors.response.use((res) => {
  // 请求成功，从队列中移除
  const { url, method, removeCache } = res.config;
  if (removeCache) removeCacheRequest(`${url}&${method}`);
}),
  (error) => {
    if (isCancel(error)) {
      // 通过CancelToken取消的请求不做任何处理
      return Promise.reject({
        message: "重复请求，自动拦截并取消",
      });
    }
  };
```

# 使用

```js
// 根据自己项目封装的axios格式来 我这里是根据我项目的第二层封装来写的
// 配置removeCache标识 则这个请求如果频繁请求 则会中断上次请求保留最新一次请求
export function engine(data) {
  return Vue.prototype.$post("/engine", data, { removeCache: true });
}
```
