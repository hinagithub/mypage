---
layout: article
title: ExpressのTips
tags: Node.js
aside:
  toc: true
---

# ExpressのTips

## ExpressでリクエストのBody情報を取得する

index.jsで`app.use(express.json())を宣言しましょう。

```js
const express = require('express');
const app = express();
app.use(express.json());
```
jsonはこれがないとエラーになるよ

```
TypeError: Cannot read property 'password' of undefined
```
