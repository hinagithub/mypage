---
layout: article
title: 【 Nuxt】v-app-bar-nav-iconを画像にする
tags: Nuxt.js Vuetify
aside:
  toc: true
---



## v-app-barでよくあるボタン

![image](https://user-images.githubusercontent.com/44778704/94163510-e9551800-fec2-11ea-8ab5-c8f1378f3016.png)

最初はこんなふうになってる
```html
<v-app-bar app clipped-left>
  <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
  <v-toolbar-title>Application</v-toolbar-title>
</v-app-bar>
```

## v-app-bar-nav-iconを画像にするには
公式を調べたらdefault slotを使うと書いてあるが、別に使わなくて良い。<br />
v-avatarを使うとキレイみたい。

```pug
 //-上部ナビゲーションバー
v-app-bar(app clipped-left)
  v-app-bar-nav-icon(@click.stop="drawer = !drawer")
    v-avatar
      img(src="/logo.png")
  v-toolbar-title(style="width: 300px" class="ml-0 pl-4")
    span(class="hidden-sm-and-down" style="color: #faebcd") Application
  v-spacer
```

参考
[VuetifyでロゴをAppBarに追加する方法](https://www.it-swarm-ja.tech/ja/vue-component/vuetify%E3%81%A7%E3%83%AD%E3%82%B4%E3%82%92appbar%E3%81%AB%E8%BF%BD%E5%8A%A0%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/807805538/)
