---
layout: article
title: 【Vuetify】v-data-tableのページネーションの表示件数を変える
tags: Vue.js Vuetify
aside:
  toc: true
---



## エラー

sassとpugをいれたらなぜかVuetifyでこんな感じのエラー。

```
Module not found: Error: Can't resolve 'core-js/modules/...' in '/app/...'
```


参考:
[nuxt.jsのbuildでcore-jsのエラーが出る](https://odaryo.hatenablog.com/entry/2019/12/10/154559)

①の方法を試してみた(core-js@2.6.11)

```
yarn add core-js@2.6.11
```

```
yarn build
```

→うまくいった!

以上
