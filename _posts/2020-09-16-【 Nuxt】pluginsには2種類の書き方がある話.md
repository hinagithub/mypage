---
layout: article
title: 【 Nuxt】pluginsには2種類の書き方がある話
tags: Nuxt.js
aside:
  toc: true
---



## pluginsの書き方

どこで呼び出すかによってpluginsの書き方が異なるらしい

### this.で呼び出す場合

- plugins/xxx.js
```js
Vue.prototype.$getLength = (word) => {
  return word.length
}
```

- components/XXX.vue
```html
<template>
  <p>this.$getLength('apple')</p>
</template>
```

### this.で呼び出せない場合

- plugins/xxx.js
```js
export default ({ app }, inject) => {
  app.$getPrefecturesJson = () => {
  }
}
```

- components/XXX.vue
```html
<template>
  <p>this.$getLength('apple')</p>
</template>
```

### 余談
nuxt.config.jsへの記載を忘れずに！

```js
plugins: [
    { src: '~/plugins/xxx.js' },
  ],
```

