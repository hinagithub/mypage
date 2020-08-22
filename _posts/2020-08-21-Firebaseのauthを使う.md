---
layout: article
title: Firebaseのauthを使う
tags: Vue.js Firebase
aside:
  toc: true
---

# はじめに

##この記事について
FirebaseのAuthoricationの使い方を知りました。
使い方を忘れてしまった未来の自分のために、設定手順などを記事にまとめておこうと思います。

参考:
[vue.jsとFirebaseでSlackクローン構築(Firebase理解編)](https://reffect.co.jp/vue/vue-js-firebase-slack-clone)
[Firebase Realtime Database公式ドキュメント](https://firebase.google.com/docs/database?hl=ja)

環境:
- @vue/cli 4.5.4
- Vue.js 3.x

※Firebaseのアカウントが必要です。

# やること

アカウント登録の際によく使われるメールリンク機能を実装します。
こんな感じです。

1. メールアドレスとパスワードで仮登録
![image](https://user-images.githubusercontent.com/44778704/90954992-8da70180-e4b4-11ea-93b0-49d813b49565.png)

2. 仮登録完了メールのリンクをクリック
![image](https://user-images.githubusercontent.com/44778704/90954376-c5ab4600-e4ae-11ea-8b08-58be6d312bbb.png)

3. 完了を確認し続行をクリック
![image](https://user-images.githubusercontent.com/44778704/90953993-46684300-e4ab-11ea-9473-3a2974fbfe10.png)

4. サインインする
![image](https://user-images.githubusercontent.com/44778704/90954401-ed9aa980-e4ae-11ea-9feb-cadf809644b1.png)

5. Homeに遷移し、「ログイン中」と表示される(メールアドレスが認証される)
![image](https://user-images.githubusercontent.com/44778704/90955547-4ec77a80-e4b9-11ea-9ba6-1de5f471979d.png)


# Vueを準備

Vue CLIでプロジェクトを作成

```
vue create プロジェクト名
```

設定項目
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
auditすると・・・
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

参考:
[Vue.js×firebaseでアプリを作ってるときにfirebaseを入れたらエラーが起きた](https://qiita.com/Yui_active/items/6b21559c2940db04b0a6)

今度はserialize-javascriptで脆弱性レポートが出る
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

npm audit fixしてもダメ。
ISSUEによると、この問題はまだ解決されていないらしい。</br>

[High vulnerability in dependencies -> copy webpack plugin -> serialize js #5782](https://github.com/vuejs/vue-cli/issues/5782)

いったん`npm-force-resolutions`でバージョン強制固定することに。</br>

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
6.0が無いから選べと言われるので4.0(最新)を選択
![image](https://user-images.githubusercontent.com/44778704/90860608-442bb900-e3c5-11ea-883d-5bddaf1b1ad7.png)

強制固定を実行

```
npx npm-force-resolutions

→npx: 5個のパッケージを3.737秒でインストールしました。
```

確認
```
npm audit
```
→ `found 0 vulnerabilities`になったらOK

参考:
[既存のnpmパッケージの依存関係を強制的に固定したいときはnpm-force-resolutionsが使える](https://scrapbox.io/nwtgck/%E6%97%A2%E5%AD%98%E3%81%AEnpm%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E3%81%AE%E4%BE%9D%E5%AD%98%E9%96%A2%E4%BF%82%E3%82%92%E5%BC%B7%E5%88%B6%E7%9A%84%E3%81%AB%E5%9B%BA%E5%AE%9A%E3%81%97%E3%81%9F%E3%81%84%E3%81%A8%E3%81%8D%E3%81%AFnpm-force-resolutions%E3%81%8C%E4%BD%BF%E3%81%88%E3%82%8B)


# Firebaseの設定

FirebaseのWebsiteからプロジェクトを作成し、RealtimeDatabaseの使用を開始する。
ルールを `true` に設定する。
![image](https://user-images.githubusercontent.com/44778704/90857764-c74a1080-e3bf-11ea-90e7-fe4e8560e244.png)

歯車アイコン→プロジェクトを設定→全般→マイアプリ→`Firebase SDK snippet`に書かれているfirebaseConfigをコピー
![image](https://user-images.githubusercontent.com/44778704/90857567-5d316b80-e3bf-11ea-8c43-9efac1faee0c.png)

main.jsに貼り付け、`import firebase from 'firebase/app'`を上部に記載します。

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';
import firebase from 'firebase/app';

var firebaseConfig = {
  apiKey: 🤫,
  authDomain: 🤫,
  databaseURL: 🤫,
  projectId: 'auth-503',
  storageBucket: 'auth-503.appspot.com',
  messagingSenderId: 🤫,
  appId: 🤫',
  measurementId: 🤫',
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);

createApp(App)
  .use(store)
  .use(router)
  .mount('#app');

```

Authenticationに追加されたことを確認
![image](https://user-images.githubusercontent.com/44778704/90865447-53166980-e3cd-11ea-9ab3-14017c449cde.png)


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


# Bootstrapの設定

インストール

```
npm install vue bootstrap-vue bootstrap
```

インポート

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';
import firebase from 'firebase/app';
import 'bootstrap/dist/css/bootstrap.css';      // add
import 'bootstrap-vue/dist/bootstrap-vue.css';  // add
```





メールテンプレートの編集
(FirebaseのE-mail認証でアドレスが正しいことを確認する)[https://nipo.sndbox.jp/develop-blog/emailverified]

ログインはできますが、emailValifiedというステータスが`false`になっています。
初回ログインをして初めてメールアドレスの確認が取れたことになるみたいです。

