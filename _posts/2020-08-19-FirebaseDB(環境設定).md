---
layout: article
title: FirebaseDB(環境設定)
tags: Vue.js Firebase
aside:
  toc: true
---

# vue-cli



# はじめに

##この記事について
Firebaseの環境設定について書いておく

[vue.jsとFirebaseでSlackクローン構築(Firebase理解編)](https://reffect.co.jp/vue/vue-js-firebase-slack-clone)
[Firebase Realtime Database公式ドキュメント](https://firebase.google.com/docs/database?hl=ja)

環境:
- @vue/cli 4.5.4
- Vue.js 3.x


# Firebase Realtime Database とは

- [NoSQL (非リレーショナル)]である
  - JOINが無い
  - CAPのうちだいたいAPかPCを満たす(RDBはCA)
    - Consistency（整合性）…常に同一のデータを参照する
    - Availability（可用性）… 常に読み出しと書き込みができる
    - Partition Tolerance（分断耐性）…ネットワークが分断されても間違った結果を引き起こさない

- mBaaS（mobile Backend as a Service）である
- データはJSONフォーマットで保存される
- クラウドデータベースである
- リアルタイムで同期される

Realtime DBをさらに強化したものが[Cloud Firestore](https://firebase.google.com/docs/database/rtdb-vs-firestore?hl=ja)。

参考:
- [【超入門】RDBとNoSQLの違いに着目！NoSQLに求めるものとは？](https://tech-blog.rakus.co.jp/entry/20180919/nosql/bigdata)
- [FirebaseのRealtime Databaseでチャットアプリを作成](https://note.com/airis0/n/n807f2e7cabea)

# 開発環境構築

以下をもとに進める
[vue.jsとFirebaseでSlackクローン構築(Firebase理解編)](https://reffect.co.jp/vue/vue-js-firebase-slack-clone)


# Vueを準備

Vue CLIでプロジェクトを作成

```
vue create プロジェクト名
```

![image](https://user-images.githubusercontent.com/44778704/90856496-02971000-e3bd-11ea-8b0f-93184687d7d4.png)

babelでエラーが出たら以下を実行。

```
npm install --save core-js
```

```js
// babel.config.js
module.exports = {
  presets: [
    '@vue/app'
  ]
}
```

参考:
[core-js/modules/es6（以下略）が見つかりません。の場合](https://qiita.com/DaisukeNishi/items/ff36054a2d00cf81aac4)

実行

```
npm run serve
```
# firebase

## import

```
npm uninstall firebase
```

## Firebase アカウント登録

手順になかったところを主に記載します。

### Realtime Databaseを選択
![image](https://user-images.githubusercontent.com/44778704/90627415-da8d9c80-e256-11ea-9bea-1002913a9e1d.png)

### ルールタブで`read`と`write`の権限を`true`に設定

```JSON
  "rules": {
    ".read": true,
    ".write": true
  }
```

[データベース ルールを使ってみる](https://firebase.google.com/docs/database/security/quickstart?hl=ja)

## .envで環境変数の設定

インストール
```
npm install dotenv
```

ルートディレクトリに`.env`ファイルを用意
ここにFirebaseへの接続情報を書く
※変数名は`VUE_APP_`から始まる必要がある

```
VUE_APP_apiKey=🤫
VUE_APP_authDomain=🤫
VUE_APP_databaseURL=https://aaaaa.com
VUE_APP_projectId=プロジェクト名
VUE_APP_storageBucket=🤫
VUE_APP_messagingSenderId=🤫
VUE_APP_appId=🤫
VUE_APP_measurementId=🤫

```

上記のキーはfirebaseのプロジェクトページからコピペ可能。

`.gitigore`に`.env`を追加する

```
// .gitignore
.env

```


[Vue.js環境変数による開発作業メモ](https://qiita.com/yoshi0518/items/f8cd408f8ef86fb02d74)


