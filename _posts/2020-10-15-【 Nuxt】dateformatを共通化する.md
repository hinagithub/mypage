---
layout: article
title: 【 Nuxt】dateformatを共通化する
tags: Nuxt.js Vuex
aside:
  toc: true
---

## 導入
### plugins/common.jsを作成

```js

import Vue from 'vue'

/* 日付フォーマット */
Vue.prototype.$dateTimeToJaDate = (dateTimeString) => {
  if (dateTimeString) {
    const date = new Date(dateTimeString)
    return `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()}`
  }
  return ''
}
```

### nuxt.config.jsに記載

```js
  plugins: [
    { src: '~/plugins/common.js', ssr: false },
    { src: '~plugins/day.js', ssr: false },
  ],
```

## 使用

```pug
  p {{ $dateTimeToJaDate(new Date()) }}
```

- 参考
[Nuxt.jsプロジェクト内で共通の処理をpluginsディレクトリにて管理する](https://designsupply-web.com/media/knowledgeside/5534/)
