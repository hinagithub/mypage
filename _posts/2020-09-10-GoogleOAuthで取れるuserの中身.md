---
layout: article
title: 【Nuxt】GoogleOAuthで取れるuserの中身
tags: Nuxt NuxtModules OAuth
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
  "picture": "Googleのアバタ画像URL",
  "email": "メールアドレス@メールアドレス.com",
  "email_verified": 真偽値,
  "locale": "ja"
}

```


あたりまえかもですが、Firebase Authenticationでは`photoURL`だった名称がこっちでは`picture`で取得するので、移行するなら名前合わせなくちゃいけないです。
<br/>
<br/>
<br/>

## 値の表示

```pug
v-img(height="150" width="40" :src="$store.state.auth.user.picture")
p {{ $store.state.auth.user.name }}
  span(v-if="is_admin" class="subtitle-2") (管理者)
p {{ me.mail_address }}
```

こんな感じになる

![image](https://user-images.githubusercontent.com/44778704/94344544-1c321400-005b-11eb-9d5d-366ebe16507d.png)


以上
