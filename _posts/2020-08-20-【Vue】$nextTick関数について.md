---
layout: article
title: 【Vue】$nextTick関数について
tags: Vue.js
aside:
  toc: true
---


# $nextTickとは

- DOMを更新した後に何かしたい時に使う関数
  - 参考:[Vue.nextTickのコードリーディング](https://aloerina01.github.io/blog/2018-09-27-1)
- `Vue.nextTick`と`this.$nextTick`がある
  - とりあえずは`this.`を使う
  - [Vue.nextTickとthis.$nextTick](https://qiita.com/murashi/items/a68a6d31f788737f0be6)
