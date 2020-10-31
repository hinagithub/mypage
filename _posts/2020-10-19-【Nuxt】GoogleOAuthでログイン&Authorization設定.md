---
layout: article
title: 【Nuxt】GoogleOAuthでログイン&Authorization設定
tags: Nuxt.js Vuex
aside:
  toc: true
---

## nuxt.config.js


```js

modules: ['@nuxtjs/auth', '@nuxtjs/axios', 'nuxt-webfontloader'],
  auth: {
    redirect: {
      login: '/login', // 未ログイン状態でアクセスした場合のリダイレクト先
      logout: '/', // ログアウト後の遷移先
      callback: '/callback', // コールバックルート
      home: '/home', // ログイン後の遷移先
    },
    strategies: {
      local: {
        token: {
          property: 'token.accessToken',
        },
      },
      localRefresh: {
        scheme: 'refresh',
        token: {
          property: 'token.accessToken',
          maxAge: 15,
        },
        refreshToken: {
          property: 'token.refreshToken',
          data: 'refreshToken',
          maxAge: false,
        },
      },
      google: {
        scheme: 'oauth2',
        authorization_endpoint: 'https://accounts.google.com/o/oauth2/v2/auth',
        userinfo_endpoint: undefined,
        scope: ['openid', 'profile', 'email'],
        client_id: process.env.GOOGLE_OAUTH_CLIENT_ID,
        client_secret: process.env.GOOGLE_OAUTH_CLIENT_SECRET,
        response_type: 'id_token', // id_token以外だとCallback.vuで固まる
        token_key: 'id_token',
        grant_type: 'authorization_code',
        access_type: 'offline',
      },
    },
  },
```

### 解説

- リダイレクト設定
  - `login: '/login'` -> 未ログイン状態でアクセスした場合のリダイレクト先
  - `logout: '/'` -> ログアウト後の遷移先
  - `callback:` '/callback' -> コールバックルート
  - `home: '/'` -> ログイン後の遷移先

- リフレッシュトークンの設定
```
local: {
        token: {
          property: 'token.accessToken',
        },
      },
      localRefresh: {
        scheme: 'refresh',
        token: {
          property: 'token.accessToken',
          maxAge: 15,
        },
        refreshToken: {
          property: 'token.refreshToken',
          data: 'refreshToken',
          maxAge: false,
        },
      },
```

ここはNuxtコミュニティのサンプルをまるっとコピペしました。
これがないと、Googleアカウントのnameとかpictureにアクセスできないので、バックエンドの設定によってはaxios実行時に常に401が返されるようになります。(そうなりました...)

[OpenID Connect](https://developers.google.com/identity/protocols/oauth2/openid-connect#java)に書いてある通り、userとかpictureにアクセスするにはリフレッシュトークンの設定が必要みたい。scopeを`['profile']`としただけではNGです。ハマった。。

>picture		The URL of the user's profile picture. Might be provided when:
The request scope included the string "profile"
The ID token is returned from a token refresh
When picture claims are present, you can use them to update your app's user records. Note that this claim is never guaranteed to be present.

Nuxtコミュニティサンプル
[nuxt-community/auth-module](https://github.com/nuxt-community/auth-module/blob/dev/demo/nuxt.config.ts)


- client_idとclient_secret

```js
  client_id: process.env.CLIENT_ID,
  client_secret: process.env.CLIENT_SECRET,
```

ここは絶対外部に出してはいけない秘密情報なのでdotenvなどの環境変数を使います。


- トークンの種類

```js
response_type: 'id_token'
token_key: 'id_token'
```

ここは用途をよく理解していないのですが、一般的にはGoogle認証を使うなら`code`にするみたいです。今回はaccess_tokenではなくid_tokenを使うのでこれなのかな？

こうしないとcallbackの画面に遷移したまま固まってindex.vueにたどり着かなくなります。


## pages/login.vue

```html
<template>
  <h1>Logging in...</h1>
</template>

<script>
export default {
  mounted() {
    this.googleAuthenticate()
  },
  methods: {
    googleAuthenticate() {
      this.$auth.loginWith('google')
    },
  },
}
</script>
```

## pages/callback.vue

```html
<template>
  <h1>Calling back...</h1>
</template>
```

nuxt.config.jsで設定したコールバックルートです。
ログイン後の画面の設定となにが違うのかよくわかってません。。

ここに記載されている説明によると、`identity provider`(認証を提供するプロバイダのことなので今回の場合はGoogle)を使ってログインしたときはこっちの画面に遷移するみたいです。
[Auth Module -redirect-](https://auth.nuxtjs.org/api/options.html#redirect)

- login: User will be redirected to this path if login is required.
- logout: User will be redirected to this path if after logout, current route is protected.
- home: User will be redirect to this path after login. (rewriteRedirects will rewrite this path)
- callback: User will be redirected to this path by the identity provider after login. (Should match configured Allowed Callback URLs (or similar setting) in your app/client with the identity provider)

じゃあ `login` の方はいらないのかも・・・？
どの項目も `false` として使用しないよにはできるみたいですがまた今度。


## plugins/axios.js

```js
import axios from 'axios'
export default function ({ $axios }) {
  $axios.onRequest((config) => {
    axios.defaults.headers.common.Authorization = localStorage.getItem('auth._token.google')
    return config
  })
}
```
`$axios.onRequest`の`axios.defaults.headers.common.XXX`に定義することで、axiosでAPI呼び出す時には毎回Headerに情報をくっつけることができるようになります。

参考にさせていただきました
[【Nuxt.js】axiosでheaderにAuthorizationを常につけたい！](https://qiita.com/masatakaaaa/items/7bc7cfb2c561c54e424a)

ログインユーザのid_tokenはローカルストレージに入っているらしい。
`localStorage.getItem('auth._token.google')`で取得できます。

pluginsはnuxt.config.jsへの定義も忘れずに。。

```js
  plugins: [
    { src: '~plugins/axios.js', ssr: false },
  ],
```

以上