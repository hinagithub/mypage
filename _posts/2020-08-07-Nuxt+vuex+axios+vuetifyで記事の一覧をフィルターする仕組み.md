---
layout: article
title: Nuxt+vuex+axios+vuetifyで記事の一覧をフィルターする仕組み
tags: 基本情報技術者試験
aside:
  toc: true
---


# この記事について
axiosでAPI取得→vuexでごにょごにょするやり方を記事にまとめます。

環境:
- Nuxt.js (@nuxt/cli v2.14.1)
- vuex
- axios
- Vuetify

# ここでやること
![image](https://user-images.githubusercontent.com/44778704/89621529-74a32b80-d8cc-11ea-8bef-c8f155c31c64.png)

- axiosで外部APIから記事一覧を取得
- その記事一覧をstoreで管理して、pageで扱う
- 特定のuserIdでフィルターする

# 1.プロジェクト作成

nuxt-cliでプロジェクトを作成します。

```
yarn create nuxt-app プロジェクト名
```

選択は以下の通りです。
![image](https://user-images.githubusercontent.com/44778704/89613608-9a750400-d8bd-11ea-9d60-93de6a632ff7.png)

ここでVuetifyの選択を忘れていたので後から追加しました。。

```
yarn add @nuxtjs/vuetify
```

```
// nuxt.config.js

  devModules: ['@nuxtjs/vuetify'],
  vuetify: {
      theme: {
        dark: false
      }
    },
```

#### 補足
devModulesはNuxt3以降で消える予定らしいです。
今回は気にせず続行します！

```WARN devModules has been renamed to buildModules and will be removed in Nuxt 3. 15:11:12```

# 結論

以下2つのファイルをつくれば上記の実装が可能です。



## `store/posts.js`
store配下に`posts.js`を作成します。

```js

// store/posts.js

import axios from "axios";

export const state = () => ({
  list: [],
  viewList: [],
  length: 0
});

export const mutations = {
  setList(state, list) {
    state.list = list;
  },
  setViewList(state, viewList) {
    state.viewList = viewList;
  },
  setLength(state, length) {
    state.length = length;
  }
};

export const actions = {
  // APIから記事を取得する
  async getList({ commit }, categories) {
    const list = await this.$axios.$get(
      "https://jsonplaceholder.typicode.com/posts"
    );

    // 記事数が100件だとサンプルとして紹介しにくいので3分の1に絞りこむ
    const oddIdList = list.filter(article => article.id % 3 === 0);

    commit("setList", oddIdList);
    commit("setViewList", oddIdList);
    commit("setLength", oddIdList.length);
  },

  // 選択されたカテゴリに一致する記事を絞り込む
  filterUser({ commit, state }, selectedId) {
    const viewList = state.list.filter(
      article => article.userId === selectedId
    );
    commit("setViewList", viewList);
    commit("setLength", viewList.length);
  }
};

```

## `pages/posts.vue`
pages配下に`posts.vue`を作成します。

```html
<!-- pages/posts.vue -->

<template>
  <v-main>
    <v-container>
      <v-row justify="center">
        <v-btn @click="filterUser(1)">user 1</v-btn>
        <v-btn @click="filterUser(2)">user 2</v-btn>
        <v-btn @click="filterUser(3)">user 3</v-btn>
        <v-col>
          <p>記事の数: {{length}}</p>
        </v-col>
      </v-row>
      <v-row>
        <v-list>
          <v-list-item v-for="(article, i) in viewList" :key="i">
            <v-list-item-content>
              <p>userId:{{article.userId}}</p>
              <v-list-item-title>{{ article.title }}</v-list-item-title>
              <v-list-item-subtitle>{{ article.body }}</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-row>
    </v-container>
  </v-main>
</template>
  <script>
import { mapActions, mapState } from "vuex";

export default {
  name: "news",
  data() {
    return {};
  },
  methods: {
    ...mapActions({
      getList: "posts/getList",
      filterUser: "posts/filterUser",
    }),
  },
  mounted: async function () {
    await this.getList(this.categories);
  },
  computed: {
    ...mapState({
      viewList: (state) => state.posts.viewList,
      length: (state) => state.posts.length,
    }),
  },
};
</script>

```

# `store/posts.jsの解説`


