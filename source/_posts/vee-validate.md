---
title: vee-validate快速入门
date: 2020-09-19 17:01:29
tags: [vuejs]
---

[vee-validate][]是[vuejs][]的表单验证库，它内置了许多规则，并且支持自定义规则。

[vuejs]: https://cn.vuejs.org/
[vee-validate]: https://logaretm.github.io/vee-validate/
[vee-validate rules]: https://logaretm.github.io/vee-validate/guide/rules.html#rules
[Handling Forms]: https://logaretm.github.io/vee-validate/guide/forms.html

## 安装

```bash
yarn add vee-validate
# OR
npm install vee-validate --save
```

## 配置

加载components并导入[rules][vee-validate rules]：

```typescript
import Vue from 'vue'
import { ValidationObserver, ValidationProvider, extend } from 'vee-validate'
import * as rules from 'vee-validate/dist/rules'
import { messages } from 'vee-validate/dist/locale/zh_CN.json'

Vue.component('ValidationObserver', ValidationObserver)
Vue.component('ValidationProvider', ValidationProvider)

for (const [rule, validation] of Object.entries(rules)) {
    extend(rule, {
        ...validation,
        message: (messages as { [key: string]: string })[rule]
    })
}
```

<!--more-->

## Usage

具体用法参见[Handling Forms][]，下面是两个常用例子：

### Basic Example

```html
<template>
  <ValidationObserver v-slot="{ invalid }">
    <form @submit.prevent="onSubmit">
      <ValidationProvider name="E-mail" rules="required|email" v-slot="{ errors }">
        <input v-model="email" type="email">
        <span>{{ errors[0] }}</span>
      </ValidationProvider>

      <ValidationProvider name="First Name" rules="required|alpha" v-slot="{ errors }">
        <input v-model="firstName" type="text">
        <span>{{ errors[0] }}</span>
      </ValidationProvider>

      <ValidationProvider name="Last Name" rules="required|alpha" v-slot="{ errors }">
        <input v-model="lastName" type="text">
        <span>{{ errors[0] }}</span>
      </ValidationProvider>

      <button type="submit" :disabled="invalid">Submit</button>
    </form>
  </ValidationObserver>
</template>

<script lang="ts">
export default {
  data: () => ({
    email: '',
    firstName: '',
    lastName: ''
  }),
  methods: {
    onSubmit () {
      alert('Form has been submitted!');
    }
  }
};
</script>

<style lang="stylus" scoped>
span {
  display: block;
}
</style>
```

### Validate Before Submit

```html
<template>
  <ValidationObserver v-slot="{ handleSubmit }">
    <form @submit.prevent="handleSubmit(onSubmit)">
      <ValidationProvider name="E-mail" rules="required|email" v-slot="{ errors }">
        <input v-model="email" type="email">
        <span>{{ errors[0] }}</span>
      </ValidationProvider>

      <ValidationProvider name="First Name" rules="required|alpha" v-slot="{ errors }">
        <input v-model="firstName" type="text">
        <span>{{ errors[0] }}</span>
      </ValidationProvider>

      <ValidationProvider name="Last Name" rules="required|alpha" v-slot="{ errors }">
        <input v-model="lastName" type="text">
        <span>{{ errors[0] }}</span>
      </ValidationProvider>

      <button type="submit">Submit</button>
    </form>
  </ValidationObserver>
</template>

<script lang="ts">
export default {
  data: () => ({
    firstName: '',
    lastName: '',
    email: ''
  }),
  methods: {
    onSubmit () {
      alert('Form has been submitted!');
    }
  }
};
</script>

<style lang="stylus" scoped>
span {
  display: block;
}
</style>
```