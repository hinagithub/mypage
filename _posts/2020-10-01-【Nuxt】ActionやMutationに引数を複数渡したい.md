---
layout: article
title: 【Nuxt】ActionやMutationに引数を複数渡したい copy
tags: Nuxt.js Vuex
aside:
  toc: true
---


## 引数1つ

### Mutations
```js
export const mutations = {
  updateItem(state, item) {
    state.item = item
  },
}
```

### Vue側
```pug
v-form(
  ref='form',
  :value='item.id',
  @input='(value) => updateRankDescriptionValidations(value)'
)
```




## 引数2つ

NGの例

### Mutations
```js
export const mutations = {
  updateItem(state, id,name) {
    state.item.id = id
    state.item.name = name
  },
}
```

### Vue側
```pug
v-form(
  ref='form',
  :value='item.id',
  @input='(value) => updateRankDescriptionValidations(value, [item.id])'
)
```

OKの例


### Mutations
```js
export const mutations = {
  updateItem(state, item) {
    state.item.id = item.id
    state.item.name = item.name
  },
}
```

### Vue側
```pug
v-form(
  ref='form',
  :value='item.id',
  @input='(value) => updateRankDescriptionValidations({value, id:[item.id]})'
)
```

ようはオブジェクトに格納して、第2引数として渡しましょうということでした。
第3引数てきなものはNG。
<br/>
<br/>
参考にさせていただきました
- [Vuex で Action, Mutation に第3引数を渡したくなったら](https://mseeeen.msen.jp/deal-with-multiple-arguments-with-action-or-mutation-in-vuex/)

