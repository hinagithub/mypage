---
layout: article
title: 【JavaScript】JSは関数内の変数定義を先頭で行うべき理由
tags: JavaScript
aside:
  toc: true
---

## 概要

JavaScriptには変数の巻き上げ(hosting)という現象があるので、関数内の変数は、関数冒頭ですえて定義すべきらしい。その理屈について本で読んだので書いておく。


## 関数内の変数は先に定義する
一般的には、プログラミングにおいて変数を定義するタイミングは「変数を使用する直前」がよいと言われている。
極端だが例えば以下の例のような感じ。
```js

function display = (message1, message2)=>{
  let prefix1 = "[message1]"
  console.log(prefix1+message1)
  let prefix2 = "[message2]"
  console.log(prefix2+message2)
}

```
しかしJavaScriptでは、変数定義は先に行うべきだと言われている。
以下のような感じで。
```js

function display = (message1, message2)=>{
  let prefix1 = "[message1]"
  let prefix2 = "[message2]"
  console.log(prefix1+message1)
  console.log(prefix2+message2)
}

これだけ短いコードならまだわかりやすいけど、長くなればなるほど、今読んでる変数がどこで定義されているのかを確認しずらくなってくる。

## なぜか

JavaScriptには、変数の巻き上げ(hosting)という概念があるかららしい。
例えば以下のようなコード。
```js
varscope = 'GlobalVariable';
functiongetValue(){
  console.log(scope); // 結果：？？？   ←❶
  varscope = 'LocalVariable';         ←❷
  return scope;
}
```

❶の出力結果は `GlobalVariable`だと思ったら間違い。
答えはUndefined。

説明:
>まずJavaScriptではローカル変数は「関数全体で有効」なので、❶の時点ではすでにローカル変数scopeが有効になっています。しかし、❶の時点ではまだローカル変数が確保されているだけで、var命令は実行されていません。つまり、ローカル変数scopeの中身は未定義（undefined）であるというわけです。

引用:
<br/>
山田 祥寛. 改訂新版JavaScript本格入門 ～モダンスタイルによる基礎から現場での応用まで (Japanese Edition) (Kindle の位置No.3444-3447). Kindle 版.


<br/>
<br/>
恐ろしい。

<br/>
<br/>
以上。
