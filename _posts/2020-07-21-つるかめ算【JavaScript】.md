---
layout: article
title: つるかめ算【JavaScript】
tags: JavaScript
aside:
  toc: true
---
 
## つるかめ算とは

以下のような問題を鶴亀算という

>*鶴と亀が合計10匹います。*<br>
*足の数は合計26本あります。*<br>
*鶴と亀はそれぞれ何匹ずついるでしょうか？*<br>

答えは以下のように総当たりで見つかる
```
// (つるの数 * つるの足の数(2本))　+  (かめの数　* かめの足の数(4本))
// (つる:1*2) + (かめ9*4) = 38 本
// (つる:2*2) + (かめ8*4) = 36 本
// (つる:3*2) + (かめ7*4) = 34 本
// (つる:4*2) + (かめ6*4) = 32 本
// ...略
// (つる:9*2) + (かめ1*4) = 22 本

```

こちらの解説が分かりやすかった。
[つるかめ算は面積図で解くな！（つるかめ算再入門）](https://www.chugakujuken.com/koushi_blog/shibata/20180517.html)


## サンプルコード

```js

const legCount = 28; // 足の数
const headCount = 10; // 頭の数
const craneLegCount = 2; // 鶴1羽の足の数
const turtleLegCount = 4; // 亀1匹の足の数

let craneCounter = 1; // つるカウンタ(0)
let turtleCounter = headCount; // かめカウンタ(10)

for (let i = 0; i < headCount - 1; i++) {
  const sum = craneLegCount * craneCounter + turtleLegCount * turtleCounter;

  if (sum === legCount) console.log(`hit! つるは${craneCounter}羽、かめは${turtleCounter}匹`);
  craneCounter++;
  turtleCounter--;
}

// 実行結果
// hit! つるは8羽、かめは3匹

```

以上

