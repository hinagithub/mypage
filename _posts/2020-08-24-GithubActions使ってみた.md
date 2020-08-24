---
layout: article
title: CircleCi使ってみた
tags: CI/CD
aside:
  toc: true
---


# CI/CDとは

>CI/CDとは「Continuous Integration／Continuous Delivery」の略で、日本語では継続的インティグレーション／継続的デリバリーといいます。CI/CDは1つの技術を指すものでなく、ソフトウェアの変更を常にテストして自動で本番環境にリリース可能な状態にしておく、ソフトウェア開発の手法を意味します。CI/CDを取り入れると、バグを素早く発見したり、変更を自動でリリースしたりできるようになります。

CI/CDツールは他にも「CircleCi」というメジャーなのがある<br />
[CircleCi使ってみた](https://hinahinako.github.io/mypage/2020/08/24/CircleCi%E4%BD%BF%E3%81%A3%E3%81%A6%E3%81%BF%E3%81%9F.html)

# CircleCiとは
- Githubが提供するCI/CDツール
- margeやmasterへのpushのタイミングでよしなにやってくれる


# 環境設定

GIthubリポジトリのActionsタブをクリックして利用開始する

![image](https://user-images.githubusercontent.com/44778704/91055805-1e6e1080-e660-11ea-816f-ab6b500af2de.png)




```yaml
# .github/workflows/node.js.yml

name: Node.js CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    # 最新のubuntuを使う
    runs-on: ubuntu-latest

    strategy:
      matrix:
        # nodeのバージョンは複数指定可能！それぞれの実行結果を得られる。
        node-version: [10.x, 12.x, 14.x]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        # さっき定義したnodeのバージョンで実行
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: npm run build --if-present
    # package.jsonで定義したtestを実行
    - run: npm test
```

以上。
CircleCiよりもあっさり設定できた。そりゃそうか。


