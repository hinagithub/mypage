---
layout: article
title: 【Nuxt】Firebase認証のダメな書き方
tags: Vue.js Vuetify
aside:
  toc: true
---

* Vuexを使ってログイン状態を管理する際の話


## これだとプロパティがないと怒られる

```js
computed: {
    ...mapState({
      isLogin: (state) => !!state.user,
      name: (state) => state.user.displayName,
      photoURL: (state) => state.user.photoURL,
    }),
  },
```


## 正しい書き方



```js


```
ちゃんとstore側で値を一つ一つ管理しましょ


