---
layout: article
title: 【Nuxt】GoogleOAuthで取れるuserの中身
tags: Vue.js Vuetify
aside:
  toc: true
---



## GoogleOAuthで取れるuserの中身

```js
{
  "sub": "数字",
  "name": "苗字名前",
  "given_name": "名前",
  "family_name": "苗字",
  "picture": "Googleのサムネイル画像",
  "email": "メールアドレス@メールアドレス.com",
  "email_verified": 真偽値,
  "locale": "ja"
}

```

あたりまえかもですが、Firebase Authenticationでは`photoURL`だった名称がこっちでは`picture`で取得するので、移行するなら名前合わせなくちゃいけないです。
<br/>
<br/>
<br/>
以上
