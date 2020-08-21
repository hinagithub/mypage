---
layout: article
title: Firebaseのauthを使う
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


Firebaseのアカウントが必要です。



# Vueを準備

Vue CLIでプロジェクトを作成

```
vue create プロジェクト名
```

![image](https://user-images.githubusercontent.com/44778704/90856496-02971000-e3bd-11ea-8b0f-93184687d7d4.png)


```
cd プロジェクト名
npm install firebase
```

脆弱性のレポートが出る

```
found 1 high severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
himac:auth hinako$ npm audit
```

```
npm audit fix
```
エラーになる
```
npm ERR! code ELOCKVERIFY
npm ERR! Errors were found in your package-lock.json, run  npm install  to fix them.
npm ERR!     Missing: core-js@^3.6.5
```

以下を実行して解決
```
npm install --save core-js
```

今度はserialize-javascriptでエラー
```
┌───────────────┬──────────────────────────────────────────────────────────────┐
│ High          │ Remote Code Execution                                        │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Package       │ serialize-javascript                                         │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Patched in    │ >=3.1.0                                                      │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Dependency of │ @vue/cli-service [dev]                                       │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Path          │ @vue/cli-service > copy-webpack-plugin >                     │
│               │ serialize-javascript                                         │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ More info     │ https://npmjs.com/advisories/1548                            │
└───────────────┴──────────────────────────────────────────────────────────────┘
```

npm audit fixしてもダメなので手動で解決する方法を模索しISSUEを発見。

[High vulnerability in dependencies -> copy webpack plugin -> serialize js #5782](https://github.com/vuejs/vue-cli/issues/5782)

この問題はまだ解決されていないらしい。。</br>
いったん`npm-force-resolutions`でバージョン強制固定することに。

package.jsonにresolutionsを追加

```json
// package.json
  "scripts": {
    // 略
  },
  "resolutions": {
    "serialize-javascript": "6.0.0"
  },
```

もういっかいインストール

```
yarn install

```
6.0が無いから選べと言われる。
![image](https://user-images.githubusercontent.com/44778704/90860608-442bb900-e3c5-11ea-883d-5bddaf1b1ad7.png)

強制固定を実行

```
npx npm-force-resolutions
```

確認
```
npm audit
```
→ `found 0 vulnerabilities`になった！


# Firebaseの設定

- FirebaseのWebsiteからプロジェクトを作成し、RealtimeDatabaseの使用を開始する。
- ルールを `true` に設定する。
![image](https://user-images.githubusercontent.com/44778704/90857764-c74a1080-e3bf-11ea-90e7-fe4e8560e244.png)

- 歯車アイコン→プロジェクトを設定→全般→マイアプリ→`Firebase SDK snippet`に書かれているfirebaseConfigをコピー
![image](https://user-images.githubusercontent.com/44778704/90857567-5d316b80-e3bf-11ea-8c43-9efac1faee0c.png)



```
npm uninstall firebase
```



実行

```
npm run serve
```
# firebase

## import

x

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


