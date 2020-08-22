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

`found 1 high severity vulnerability`がでたらこちらを参照して下さい。
[VueでFirebaseをインストールするときに出るhigh severity vulnerability](https://hinahinako.github.io/mypage/2020/08/21/Vue%E3%81%A7Firebase%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B%E3%81%A8%E3%81%8D%E3%81%AB%E5%87%BA%E3%82%8Bhigh-severity-vulnerability.html)

# Firebaseの設定

こちらを参考にプロジェクトを作成します。
[vue.jsのログイン認証をFirebase Authenticationを使って構築](https://reffect.co.jp/vue/vue-js-authentication-by-firebase)


設定が完了すると、`main.js`が以下のようになっているはずです。
```js:main.js
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




## router/index.js

router/index.jsを修正し、`/register`と`/signin`へのルーティングを設定します。

```js
import { createRouter, createWebHistory } from 'vue-router';
import Home from '../views/Home.vue';
import Register from '../views/Register.vue';
import Signin from '../views/Signin.vue';

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home,
  },
  {
    path: '/register',
    name: 'Register',
    component: Register,
  },
  {
    path: '/signin',
    name: 'Signin',
    component: Signin,
  },
];

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
});

export default router;

```

## App.vue

App.vueもルーティングにあわせて修正します。

```html
<template>
  <div id="app">
    <div id="nav" class="p-2">
      <router-link to="/">Home</router-link>|
      <router-link to="/register">Register</router-link>|
      <router-link to="/signin">Signin</router-link>
    </div>
    <router-view />
  </div>
</template>
```


## views/Home.vue

ログイン後に遷移する画面です。

```js
<template>
  <div class="w-25 p-5">
    <p class="h4 pb-3">HOME</p>
    <p>ログイン状態: {{ authState }}</p>
    <p>メール認証: {{ emailVerified }}</p>
    <button class="btn btn-secondary" @click="signOut">サインアウト</button>
  </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/auth";

export default {
  name: "Home",
  data() {
    return {
      authState: "",
      emailVerified: "",
    };
  },
  methods: {
    signOut() {
      firebase.auth().signOut();
    },
  },
  mounted() {
    firebase.auth().onAuthStateChanged((user) => {
      console.log(user);
      if (user) {
        this.authState = "ログイン";
        this.emailVerified = user.emailVerified ? "済" : "未";
      } else {
        this.authState = "ログアウト";
        this.emailVerified = "-";
      }
    });
  },
};
</script>
```

- `.onAuthStateChanged()`: で現在ログインしているユーザの情報を取得することができます。
- `user.emailVerified`プロパティ: メール認証をしているかどうかがわかります。


補足:
アカウント作成の際、仮登録完了メールを受け取っただけで、まだメール内に書かれているリンクをクリックしていない場合でもログインは可能です。 `user.emailVerified`は、メール認証のリンクをクリックし、その後初回ログインをしてはじめて`true`になるみたいです。

参考:
[Vue.jsとFirebaseで認証メール機能(vuetifyおまけ付き)](https://note.com/tenlife/n/nb225fc1269c7)



メールテンプレートの編集
(FirebaseのE-mail認証でアドレスが正しいことを確認する)[https://nipo.sndbox.jp/develop-blog/emailverified]

ログインはできますが、emailValifiedというステータスが`false`になっています。<br/>
初回ログインをして初めてメールアドレスの確認が取れたことになるみたいです。


