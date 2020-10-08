---
layout: article
title: 【 Nuxt】dayjsの導入
tags: Nuxt.js
aside:
  toc: true
---

## 導入

### dayjsのインストール

```
npm i dayjs
```

### pluginsファイルを用意

`/plugins/day.js`を作成し以下をコピペ
```js
import dayjs from 'dayjs'

export default ({ app }, inject) => {
   inject('dayjs', ((string) => dayjs(string)))
}
```

### nuxt.config.jsに記載

```
  plugins: [
    { src: '~plugins/day.js', ssr: false },
  ],
```

または

  ```
    plugins: [
    '~plugins/day.js',
  ],
```


## 使い方

### Storeやmountedで使用

`this.`が必要

```js
const today = this.$dayjs().format('YYYY-MM-DD')
console.log(today)

// ->2020-10-08
```

### <template>で使用(pug)

`this.`は不要

```pug
p {{ $dayjs().format('YYYY-MM-DD') }}
```

参考にさせていただきました

[【Nuxt.js】dayjs導入編：リアルタイムな日時を表示してみよう](https://note.com/aliz/n/nbd10153f563f)

