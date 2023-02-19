---
layout: article
title: 2023-02-19-React+PokeAPIでポケモン検索ページを作る
tags: React
aside:
  toc: true
---



# 🌱 初めに

VueだけじゃなくてReactも触ってみようかな。
と思い立ってシンプルな検索ページを作ってみました。

<video controls playsinline width="100%" autoplay loop muted="true" src="https://user-images.githubusercontent.com/44778704/219949773-9e541b7a-8515-4815-a5f9-dcd6bb9dd5fd.mp4" type="video/mp4" >
 Sorry, your browser doesn't support embedded videos.
</video>

# ♻️ 環境
- Node.js: v19.6.0

# 🗂 ディレクトリ構成

```
.
├── README.md
├── node_modules
│   ├── 略
├── build
│   ├── 略 # ビルド後のソースが置かれる場所
├── docs
│   ├── 略 # GithubPagesで公開するために必要
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── manifest.json
│   └── pikachu.png
├── src
│   ├── App.tsx
│   ├── api
│   │   └── pokemon.ts
│   ├── components
│   │   ├── IconBtn.tsx
│   │   ├── ItemCard.tsx
│   │   ├── PokemonList.tsx
│   │   └── Search.tsx
│   ├── index.tsx
│   └── types
│       └── pokemon.ts
├── package.json
├── package-lock.json
└── tsconfig.json
```

# 🗂 コンポーネント構成

大きなページとして`PokemonList.tsx`があって、
小さなパーツのコンポーネントとして
- Search.tsx
- IconBtn.tsx
- ItemCard.tsx

を作りました。

![pokelist_struct](https://user-images.githubusercontent.com/44778704/219954122-161c04b1-70c3-4ac0-888f-2aac6460ba96.jpg)

# 👩‍🏫 解説

### カードのリストを表示するところ

何よりもまずポケモンのカードを作って並べるところから始めました。

```js
<Box
  sx={{
    display: 'flex',
    flexDirection: 'row',
    flexWrap: 'wrap',
    justifyContent: 'space-evenly',
    p: 0,
    mx: 1,
  }}
>
  {pokemons.map((pokemon, i) => (
    <div style={{ margin: 10, padding: 10 }} key={i}>
      <ItemCard
        id={pokemon.id}
        name={pokemon.name}
        url={pokemon.url}
        types={pokemon.types}
      ></ItemCard>
    </div>
  ))}
</Box>
```

最初はグリッドレイアウトを使っていましたが、ブレークポイントを細かく設定しないと、画面のサイズによってはカードが重なったり、離れすぎたりして...やめました。
MUIのブレークポイントはこんな感じです

```
xs, extra-small: 0px
sm, small: 600px
md, medium: 900px
lg, large: 1200px
xl, extra-large: 1536px
```

`justifyContent`で設定する並び方はcenterにするか、space-aroundにするかの細かいところに悩んだりしました。

- `justifyContent: 'space-evenly'`
<img width="1221" alt="image" src="https://user-images.githubusercontent.com/44778704/219955419-b807dff2-4d7e-4a3f-b734-ad03aca11276.png">


- `justifyContent: 'center'`
<img width="1224" alt="image" src="https://user-images.githubusercontent.com/44778704/219955492-1807c56e-059b-41fa-bd9b-b47e98f24c23.png">


## カード

まずGridで二つに分解して、

<img width="299" alt="image" src="https://user-images.githubusercontent.com/44778704/219955682-8b2da559-fb54-4cb5-b1ee-3a5444c3841b.png">

右側はなんとか赤い線の枠内に並ぶように作っています。

タイプはMax2を想定して作っているので、それ以上が来たら多分レイアウト崩れます。

アルセウスとかどうなってるんだろう。。

<img width="436" alt="image" src="https://user-images.githubusercontent.com/44778704/219955975-1af2c4b4-df03-42a1-98b7-bf6802e64361.png">



# 📘 参考

参考にさせていただきました
- [Node.js v18.6.0 documentation](https://nodejs.org/api/crypto.html#class-cipher)
- [runebook.dev日本語 Crypto](https://runebook.dev/ja/docs/node/crypto)
- [セッション認証とトークン認証について調べたことをまとめる](https://zenn.dev/pictogram/scraps/f8fc8d80ffc3be)
- [ASCII文字とURLエンコードの対応表](https://www.seil.jp/doc/index.html#tool/url-encode.html)


# 🔑 終わりに
暗号は奥が深いし認証の種類も複数あるし、どこからどこまで調べたらいいのかわからずネットの海を彷徨いました。
コードを書いて動かして、「こうしたらどうなる？」という実験をしながら調べたら、意外と理解しやすかったです。
そんな気がするだけかな。。


以上




