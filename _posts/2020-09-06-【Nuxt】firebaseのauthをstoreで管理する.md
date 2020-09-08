---
layout: article
title: 【Nuxt】firebaseのauthをstoreで管理する
tags: Vue.js Vuetify
aside:
  toc: true
---

## やりたいこと

- firebaseのGoogleログインを使用する
- loginボタンなどはない。URLにアクセスしたら即Googleログイン認証を開始する
- ログイン状態はstoreで管理する
- middlewareでページ遷移のたびにログイン状態を確認する




## .env
```

    apiKey = 🤫
    authDomain = 🤫
    databaseURL = 🤫
    projectId = 🤫
    storageBucket = 🤫
    messagingSenderId = 🤫
    appId = 🤫
    measurementId = 🤫

```

## nuxt.config.js

```js
require("dotenv").config();
const {
  apiKey,
  authDomain,
  databaseURL,
  projectId,
  storageBucket,
  messagingSenderId,
  appId
} = process.env;

export default {
  /*
   ** Nuxt rendering mode
   ** See https://nuxtjs.org/api/configuration-mode
   */
  mode: "spa",

  /*
   ** Nuxt middlewareをどこでも使えるように設定
   */
  router: {
    middleware: ["authenticated"]
  },
  /*
   ** Nuxt target
   ** See https://nuxtjs.org/api/configuration-target
   */
  target: "server",
  /*
   ** Headers of the page
   ** See https://nuxtjs.org/api/configuration-head
   */
  head: {
    title: process.env.npm_package_name || "",
    meta: [
      { charset: "utf-8" },
      { name: "viewport", content: "width=device-width, initial-scale=1" },
      {
        hid: "description",
        name: "description",
        content: process.env.npm_package_description || ""
      }
    ],
    link: [{ rel: "icon", type: "image/x-icon", href: "/favicon.ico" }]
  },
  /*
   ** Global CSS
   */
  css: ["firebaseui/dist/firebaseui.css"],
  /*
   ** Plugins to load before mounting the App
   ** https://nuxtjs.org/guide/plugins
   */
  plugins: ["~/plugins/firebase.js", "~/plugins/fireauth.js"],
  /*
   ** Auto import components
   ** See https://nuxtjs.org/api/configuration-components
   */ components: true,
  /*
   ** Nuxt.js dev-modules
   */
  buildModules: ["@nuxtjs/vuetify"],
  vuetify: {
    theme: {
      dark: false,
      themes: {
        light: {
          main: "#434343",
          secondary: "#79736A",
          primary: "#CAA667",
          accent: "#F7C873",
          sub: "#FAEBCD",
          error: "#e74c3c",
          action: "#23DB2A"
        }
      }
    }
  },
  /*
   ** Nuxt.js modules
   */
  modules: [
    // Doc: https://axios.nuxtjs.org/usage
    "@nuxtjs/axios"
  ],
  /*
   ** Axios module configuration
   ** See https://axios.nuxtjs.org/options
   */
  axios: {},
  /*
   ** Build configuration
   ** See https://nuxtjs.org/api/configuration-build/
   */
  build: {},
  env: {
    apiKey,
    authDomain,
    databaseURL,
    projectId,
    storageBucket,
    messagingSenderId,
    appId
  }
};

```

## plugins/firebase.js

```js
import firebase from "firebase/app";
import "firebase/auth";

if (!firebase.apps.length) {
  firebase.initializeApp({
    apiKey: process.env.apiKey,
    authDomain: process.env.authDomain,
    databaseURL: process.env.databaseURL,
    projectId: process.env.projectId,
    storageBucket: process.env.storageBucket,
    messagingSenderId: process.env.messagingSenderId
  });
}

export const auth = firebase.auth();
export default firebase;

```

## plugins/fireauth.js
```js
import firebase from "~/plugins/firebase";

function auth() {
  return new Promise((resolve, reject) => {
    firebase.auth().onAuthStateChanged(user => {
      resolve(user || false);
    });
  });
}
export default auth;

```

## middleware/authenticated.js

```js
import firebase from "~/plugins/firebase";
export default function({ route, store }) {
  // ログイン済の場合は何もしない
  if (store.state.auth.isAuth) return;

  // ログインしてない場合、ログインステータスを確認
  firebase.auth().onAuthStateChanged(user => {
    if (user) {
      // ユーザ情報があればstoreを更新
      store.dispatch("auth/checkAuth");
    } else {
      // ユーザ情報がなければログインさせる
      store.dispatch("auth/signIn");
    }
  });
}

```


## store/auth.js
```js
import firebase from "~/plugins/firebase";
import fireauth from "~/plugins/fireauth";

export const state = () => ({
  isAuth: false,
  displayName: "",
  email: "",
  photoURL: ""
});

export const mutations = {
  setSignInState(state, user) {
    state.isAuth = !!user;
    state.email = (user && user.email) || "";
    state.displayName = (user && user.displayName) || "";
    state.photoURL = (user && user.photoURL) || "";
  }
};

export const actions = {
  async signIn({ commit }) {
    console.log("signIn called");
    await firebase
      .auth()
      .signInWithRedirect(new firebase.auth.GoogleAuthProvider())
      .then(res => commit("setSignInState", res.user))
      .catch(error => console.error(error));
  },
  async signOut({ commit }) {
    await firebase
      .auth()
      .signOut()
      .then(res => {
        commit("setSignInState", false);
      });
    window.close();
  },
  async checkAuth({ commit }) {
    await fireauth().then(user => commit("setSignInState", user));
  }
};

```


## pages/index.vue

```html
<template>
  <div>
    <p>/index.vue</p>
    <p v-if="isAuth">ようこそ、{{name}}さん</p>
  </div>
</template>

<script>
import firebase from "@/plugins/firebase";
import { mapActions, mapState } from "vuex";

export default {
  components: {},

  methods: {
    ...mapActions({
      getName: "me/getName",
    }),
  },
  mounted: function () {
    this.getName();
  },

  computed: {
    ...mapState({
      isAuth: (state) => state.auth.isAuth,
      displayName: (state) => state.auth.displayName,
      email: (state) => state.auth.email,
      photoURL: (state) => state.auth.photoURL,
      name: (state) => state.me.name,
    }),
  },
};
</script>

```


