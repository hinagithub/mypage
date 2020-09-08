---
layout: article
title: 【Vuetify】v-data-tableのページネーションの表示件数を変える
tags: Vue.js Vuetify
aside:
  toc: true
---


## v-data-tableのページネーションの表示件数を変える


```html
<v-data-table
   ....
   :footer-props="{'items-per-page-options':[2,15, 30, 50, 100, -1]}"
   :options="options">
   ....
</v-data-table>
...
data() {
  return {
    options: {
      itemsPerPage: 100
    }, ...
  }
}
```

これでもいけるみたい。

```html
<v-data-table
:footer-props="{'items-per-page-options':[15, 30, 50, 100, -1]}"
>
```



[Stack Overflow -How to set initial 'rows per page' value in Vuetify DataTable component?-](https://stackoverflow.com/questions/55410879/how-to-set-initial-rows-per-page-value-in-vuetify-datatable-component)
