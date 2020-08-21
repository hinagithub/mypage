---
layout: article
title: 【Vue】Vue-cliでbootstrapを使えるようにする
tags: Vue.js
aside:
  toc: true
---

## npm install

```
npm install vue bootstrap-vue bootstrap
```
※5分くらいかかる


## index.js

bootstrapの記述を加える

```js
import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue' // added
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

Vue.use(Router)
Vue.use(BootstrapVue) // added

import 'bootstrap/dist/css/bootstrap.css' // added
import 'bootstrap-vue/dist/bootstrap-vue.css' // added

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    }
  ]
})
```

以上。
こちらの記事をそのまま実行すれば動いた。

[Vue.js + Bootstrap4でポートフォリオサイトの雛形を作ろう！](https://qiita.com/masa08/items/3474d97a1283dfc275c9)
