---
layout: article
title: VueでNODE_ENVの動きを確かめる
tags: Vue.js
aside:
  toc: true
---



## process.env.NODE_ENVとは

本番環境と開発環境で環境変数を分けたい場合に使う。


## NODE_ENVの挙動

デフォルトでプロジェクトを作成
```
vue create envtest
```

プロジェクトのルート配下に`vue.config.js`を配置

```
console.log(process.env.NODE_ENV)
```

これで `serve` を実行してみる

```
npm run serve
```

実行結果
``` cmd
$ npm run serve

> envtest@0.1.0 serve /Users/hinako/Vue/envtest
> vue-cli-service serve

NODE_ENVの値:  development
 INFO  Starting development server...
98% after emitting CopyPlugin

 DONE  Compiled successfully in 2609ms                                                  23:42:50
```

>NODE_ENVの値:  development

NODE_ENVはcrossenvのプラグインとしてでしか使えないと思っていたが、デフォルトで使用できる値でした。

`vue.config.js`は初期値を設定するファイルなので、ビルドする前に実行されるみたいです。

`npm run serve`は開発としての実行コマンドなので、値は`development`になっています。

じゃあ本番用のコマンドである`npm run build`を叩いたらどうなるかと
うと・・

```
$ npm run build
> envtest@0.1.0 build /Users/hinako/Vue/envtest
> vue-cli-service build

NODE_ENVの値:  production

⠋  Building for production...
```

>NODE_ENVの値:  production


こうなりました。便利だ。

## 環境変数の設定

このNODE_ENVの仕組みを利用して本番用と開発用の環境変数を使い分けることが可能です。
