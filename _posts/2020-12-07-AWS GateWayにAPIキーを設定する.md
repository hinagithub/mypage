b---
layout: article
title: AWS GateWayにAPIキーを設定する
tags: AWS
aside:
  toc: true
---

こちらの記事を参考にAWS GatewayにAPIキーを設定します。
[ゼロから作りながら覚えるAPI Gateway環境構築](https://dev.classmethod.jp/articles/getting-start-api-gateway/)


### APIキーの作成

API Gatewayの左側のメニューにある「APIキー」をクリックし、アクション>APIキーの作成を選択します。
キーの名前はなんでもOK

![image](https://user-images.githubusercontent.com/44778704/101315142-54e13b80-389d-11eb-9d5b-602d76ddf7f1.png)


### 使用量プランの作成

API Gatewayの左側のメニューにある「使用量プラン」をクリックし、「作成」をクリックします。

ここでは、リクエスト数が大きくならないようにの設定で制限を設けることができる。

(APIキーを使う場合は必ずこの使用プランを作成して割り当てる必要がある)

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

### 実行してみる
Postmanで実行してみます
![image](https://user-images.githubusercontent.com/44778704/101314872-ca98d780-389c-11eb-839b-f0d1dedd46e4.png)

レスポンスステータスが200で、Bodyに`Hello XXX`と返ってきているので成功です。
ちなみに、APIキーを一文字でも変えて実行するとレスポンスステータスはきちんと`403`(権限がない)のエラーになります。

</br>
</br>
以上
