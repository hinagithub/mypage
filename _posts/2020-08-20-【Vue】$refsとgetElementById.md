---
layout: article
title: 【Vue】$refsとgetElementById
tags: Vue.js
aside:
  toc: true
---


# $refsとは

- HTMLタグに `$refs="ref名"`を書いておけば、methodなどからref名を指定して値を取れる。
- getElementByIdでidをもとにDOMを動かすのとおなじ。
- DOM構成前に指定すると(created内とか)もちろんアクセスできないので注意
- idと違うのは、複数あった時は配列で取得できる点。

```HTML
  <input ref="focusThis">
  <button @click="() => this.$refs.focusThis.focus()">
    ここをクリックしてね
  </button>

<script>
 new Vue({
    el: "#app",
    methods: {
      focusInput() {
        this.$refs.focusThis.focus();
      },
    },
  })
</script>
```

参考:
[[Vue] 任意のタグにフォーカスする際などに、$refsが最適すぎた件
](https://qiita.com/riotam/items/f5b4fe26d28034e14cb7)
