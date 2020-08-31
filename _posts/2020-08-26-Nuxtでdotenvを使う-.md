---
layout: article
title: Nuxtでdotenvを使う
tags: Vim
aside:
  toc: true
---

## インストール

```
npm install @nuxtjs/dotenv
```

## nuxt.config.jsの設定

```js
require("dotenv").config();
const {
  apiKey,
  authDomain,
  databaseURL,
  projectId,
  storageBucket,
  messagingSenderId,
  appId
} = process.env;

export default {

  // 略

  build: {},
  env: {
    apiKey,
    authDomain,
    databaseURL,
    projectId,
    storageBucket,
    messagingSenderId,
    appId
  }
};
```

## .env
ルートディレクトリに`.env`ファイルを作成して以下を記述

```json
// .env

    apiKey = 🤫;
    authDomain =  🤫;
    databaseURL = 🤫;
    projectId = "projectname-503"
    storageBucket = "projectname-503.appspot.com"
    messagingSenderId =  🤫;
    appId =  🤫;
    measurementId =  🤫;
```


## 参考
[Nuxt.jsにおけるenvファイルの利用(初学者向けハンズオン)](https://qiita.com/fj_yohei/items/c77bff6f0177b4ff219e)


以上
