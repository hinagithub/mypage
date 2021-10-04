---
layout: article
title: 【JWT】payload予約済みパラメータの意味
tags: JWT
aside:
  toc: true
---
JWT(Json Web Token)のpayload(属性情報)の予約済みパラメータの意味をいつも忘れてしまうのでメモ。

参考:

- [JWT draft-ietf-oauth-json-web-token-11](https://openid-foundation-japan.github.io/draft-ietf-oauth-json-web-token-11.ja.html#ReservedClaimName)

- [IDトークンが分かれば OpenID Connect が分かる](https://qiita.com/TakahikoKawasaki/items/8f0e422c7edd2d220e06#7-id-%E3%83%88%E3%83%BC%E3%82%AF%E3%83%B3)

### iss (issurer)
発行者

### sub (subject)
ユーザを一意に特定する値(DBだとprimary keyとか)

### aud (audience)
依頼者(トークンの発行を依頼したクライアントのクライアントID)

### exp (expiration time)
JWTの有効期限

### iat (issued at)
JWT発行日時

### auth_time
認証時刻

### acr (authentication context class reference)
認証に使用したクラス(パスワード `urn:oasis:names:tc:SAML:2.0:ac:classes:Password` など)

### nonce (number used once) (多分)
リプレイアタックを阻止するためにJWTに含める値

### amr (authentication methods references)
認証手法

### azp(authorized party)
認可された対象者


以上
今後設定例とかを追記していこうかな
