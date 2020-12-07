---
layout: article
title: AWS GateWayにAPIキーを設定する
tags: AWS
aside:
  toc: true
---

こちらの記事を参考にAWS GatewayにAPIキーを設定します。
[ゼロから作りながら覚えるAPI Gateway環境構築](https://dev.classmethod.jp/articles/getting-start-api-gateway/)




### 使用

リクエスト数が大きくならないように、「使用量プラン」の設定で制限を設けることができる

![image](https://user-images.githubusercontent.com/44778704/101306270-c7472100-3887-11eb-98c4-1a0cbc68ad54.png)


- スロットリング
  - レート・・・1秒あたりのリクエスト数(例では1秒に2回までに設定)
  - バースト・・・リクエストの同時実行数(例では50回までに設定)
- クォータ
  - 期間内に実行できる回数(例では1日100回までに設定)


[API GatewayのAPIキーと使用量プランについて調べてみた](https://dev.classmethod.jp/articles/try-api-gateway-usage-plan/)


### プランの関連付け

この使用プランを適応したいAPIを設定する。
メソッドスロットリングを設定すると、メソッド単位でキーの要否を設定できそう。今回は割愛。

![image](https://user-images.githubusercontent.com/44778704/101306382-14c38e00-3888-11eb-856d-255252d955c4.png)

最初に作ったmyKeyを割り当てて完了。

![image](https://user-images.githubusercontent.com/44778704/101306590-a16e4c00-3888-11eb-8747-deeaa04eee65.png)


