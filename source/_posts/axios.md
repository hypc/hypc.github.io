---
title: Axios
date: 2021-06-04 14:46:09
tags:
---

[Axios][]是一个基于`promise`的网络请求库，可以用于浏览器和`node.js`。[Axios][]使用简单，包尺寸小且提供了易于扩展的接口。

[Axios]: https://axios-http.com/

```typescript
import axios from "axios";
axios.get('/users').then(res => {
  console.log(res.data);
});
```

## 特性

* 从浏览器创建 XMLHttpRequests
* 从 node.js 创建 http 请求
* 支持 Promise API
* 拦截请求和响应
* 转换请求和响应数据
* 取消请求
* 自动转换JSON数据
* 客户端支持防御XSRF

<!--more-->

## 简单封装

```typescript
import 'reflect-metadata';
import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios';

// eslint-disable-next-line @typescript-eslint/no-explicit-any
declare type Constructor<T = any> = new (...args: any[]) => T
declare type AxiosOptions = {
  config: AxiosRequestConfig;
  interceptors?: {
    request?: ((config: AxiosRequestConfig) => AxiosRequestConfig)[];
    response?: ((resp: AxiosResponse) => AxiosResponse)[];
  };
}

const Agents = (target: Constructor): Constructor => {
  return class extends target {
    constructor () {
      super();
      const instances = Reflect.getMetadata('axios:instances', this) as string[] || [];
      instances.forEach((name) => {
        const options = Reflect.getMetadata('axios:options', this, name) as AxiosOptions;
        const instance = axios.create(options.config);
        if (options.interceptors?.request) {
          options.interceptors.request.forEach(c => instance.interceptors.request.use(c));
        }
        if (options.interceptors?.response) {
          options.interceptors.response.forEach(c => instance.interceptors.response.use(c));
        }
        Object.defineProperty(this, name, { value: instance });
      });
    }
  };
};

const Agent = (options: AxiosOptions): PropertyDecorator => (target, propertyKey) => {
  const instances = Reflect.getMetadata('axios:instances', target) as string[] || [];
  instances.push(propertyKey as string);
  Reflect.defineMetadata('axios:instances', instances, target);
  Reflect.defineMetadata('axios:options', options, target, propertyKey);
};

// example
@Agents
class AxiosAgents {
  @Agent({
    config: {
      baseURL: 'https://api.google.com/'
    }
  })
  public google!: AxiosInstance;

  @Agent({
    config: {
      baseURL: 'https://api.baidu.com/'
    }
  })
  public baidu!: AxiosInstance;
}

export default new AxiosAgents();
```
