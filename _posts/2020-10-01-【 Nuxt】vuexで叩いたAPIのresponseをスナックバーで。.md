---
layout: article
title: 【 Nuxt】vuexで叩いたAPIのresponseをスナックバーで。
tags: Nuxt.js Vuex
aside:
  toc: true
---


## やりたいこと
VuexでupdateやdeleteとかのAPIを叩いた後、その結果(成功/エラー)をスナックバーで表示するあれ。

![image](https://user-images.githubusercontent.com/44778704/94774022-f361a400-03f7-11eb-8c32-b32c9b9c43d5.png)

`errMessage`というstateを作って管理してみる

## usersテーブルの例

### store側

```js

// store/users.js

export const state = () => ({
  users: [],
  errMessage: '',
})

export const mutations = {
  errMessage(state, errMessage) {
    state.errMessage = errMessage
  },
}

 async updateUser({ commit }, item) {
    const body = {
      id: item.id,
      name: item.name,
    }
    await this.$axios
      .$put(`/users/${item.id}`, body)

        // 成功した場合は空文字を格納
      .then(function (response) {
        commit('setErrMessage', '')
      })

      // エラーになった場合はエラーをそのまま格納
      .catch(function (error) {
        commit('setErrMessage', error)
      })
  },

```


### Vue側

```pug

//- Users.vue

<template lang="pug">
div
  v-data-table.elevation-1(
    :items='users',
  )

  //- この辺少しテキトウ。本当はsave関数にupdateしたいitemを渡す。
  .text-center.ma-2
    v-btn(@click="save") Save

    //- 成功スナックバー
    v-snackbar(v-if='!errMessage', v-model='snackbarSuccess',color='success')
      span
        v-icon.mr-3 mdi-check-circle
      span 成功
      template(v-slot:action='{ attrs }')
        v-btn(color='white', text, small, v-bind='attrs', @click='snackbarSuccess = false')
          v-icon(small) mdi-window-close

    //- 失敗スナックバー
    v-snackbar(v-else, v-model='snackbarError', color='error', timeout=-1)
      span
        v-icon.mr-3 mdi-alert-circle
      span エラー : {{ errMessage }}
      template(v-slot:action='{ attrs }')
        v-btn(color='white', text, small, v-bind='attrs', @click='snackbarError = false')
          v-icon(small) mdi-window-clos
</template>

<script>
import { mapActions, mapState } from 'vuex'

export default {
  data: () => ({
    snackbarSuccess: false,
    snackbarError: false,
  }),

  computed: {
    ...mapState({
      users: (state) => state.users.users,
      errMessage: (state) => state.users.errMessage,
    }),
  },

  async mounted() {
    await this.getUsers()
    await this.getRankSelectOptions()
  },

  methods: {
    ...mapActions({
      getUsers: 'users/getUsers',
      updateUser: 'users/updateUser',
    }),

    async save() {
      await this.updateUser(item)
      await this.getUsers()

      if (!this.errMessage) {
          this.snackbarSuccess = true
        this.close()
      } else {
          this.snackbarError = true
      }
    },
  },
}
</script>

```


ここで↓`save()`内で使ってる関数をawaitにしているのが大事なポイント。
```
    async save() {
      await this.updateUser(item)
      await this.getUsers(
```
これをしないと、updateが実行されてステータスを取ってくるよりも前に`this.snackbarSuccess = true`ので、成功と失敗のスナックバー描画が正しく表示されなくなる。
<br/><br/>
Actions内でasync/awaitしているから大丈夫だと思ったら大間違いでした。
<br/><br/>

以上
