---
layout: article
title: 【JavaScript】二重否定!!
tags: JavaScript
aside:
  toc: true
---

## !!obj

```js

const obj = {"りんご":"赤"}
if(!!obj){
  // 処理
}

```

`(!obj)`はundefinedなら処理をしないお決まりの書き方だが、こに対応していないブラウザがあるらしい。<br/>
そういう時は`!!(obj)`で「`true`なら処理する」という書き方にするとGood。今時は必要のない書き方。
<br/>
<br/>
へぇ〜
<br/>
<br/>

参考
[【JavaScript】!!（ビックリマーク2つ）って何？](https://senews.jp/bikkuri-2/)
