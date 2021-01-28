---
layout: article
title: AWS LambdaでAPI作成
tags: AWS
aside:
  toc: true
---

こちらの記事を参考にAWS LambdaでサーバレスAPIを作ってみます
[【入門】LambdaとAPI Gatewayの使い方](https://www.wakuwakubank.com/posts/519-aws-lambda-introduction/)

# Lambdaの設定

## 関数の作成

以下の情報を入力して関数を作成

- 一から作成
- 関数名: myFunction
- Node.js 12.x
- デフォルトの実行ロールの変更
  - 基本的なLambdaアクセス権限で新しいロールを作成

![image](https://user-images.githubusercontent.com/44778704/101112053-d7f55e00-361f-11eb-9373-f4b4d357549c.png)


作成ボタンをクリックして完了

## 関数コードを編集する

以下のコードを関数コード(index.js)に貼り付ける

```js
exports.handler = async (event) => {
    console.log(event)
    console.log(process.env)

    const response = {
        statusCode: 200,
        body: 'Hello',
    }
    return response
}
```

![image](https://user-images.githubusercontent.com/44778704/101113067-ee041e00-3621-11eb-9970-7df8f0305917.png)


## テストする
右上のテストボタンをクリック。
イベント名のみ入力。もう一度テストボタンをクリックするとテストが実行される。
![image](https://user-images.githubusercontent.com/44778704/101112566-f445ca80-3620-11eb-9c71-22251ed820b4.png)

実行結果
```json
{
  "statusCode": 200,
  "body": "Hello"
}
```


# Gatewayの設定

- 新しいAPIの作成をクリック
- REST APIを選択(プライベートではない方)

## リソースとメソッドの作成

- `/`を選択した状態でアクションから「リソースの作成」を選択
- `message` というリソースを作成する
- /messageを選択した状態でアクションから「メソッドの作成」を選択
- 「GET」を選んで✅で決定する
- Lambda関数に✅をつけ、関数名は先ほど作成した「myFunction」にする

![image](https://user-images.githubusercontent.com/44778704/101113555-f872e780-3622-11eb-97da-3543eb9d0314.png)


- 最後に、アクションからAPIのデプロイをクリックする。初回の場合は新しいステージ名をつける。
![image](https://user-images.githubusercontent.com/44778704/103476784-ffe10880-4dfb-11eb-9c87-e5bdde27289a.png)


## 実行

APIのデプロイが完了するとURLが発行されるのでコピーする
![image](https://user-images.githubusercontent.com/44778704/101114977-e21a5b00-3625-11eb-9c31-f722e9820909.png)


### curlで実行

```
 curl https://XXXXX.execute-api.ap-northeast-1.amazonaws.com/dev/message

```

### ブラウザで実行

URLにアクセスするだけ。
![image](https://user-images.githubusercontent.com/44778704/101115222-548b3b00-3626-11eb-974d-795e27cc87b5.png)


### 補足


- APIのデプロイは忘れがちなので注意
  - [aws API Gatewayデプロイ時に、「 No integration defined for method」エラーの対策](https://kaoru2012.blogspot.com/2017/08/aws-api-gateway-no-integration-defined.html)


- `/message`をつけるのを忘れずに！
  - つけないとルートにアクセスすることになり、403エラー(`{"message":"Missing Authentication Token"}`)になる
  - [API Gateway {“message”:”Missing Authentication Token”} が返ってきた時](https://bokuranotameno.com/post-10884/)



# Lambda実行時に引数を渡す

現時点でCloud Watchには `event`の内容は空になっています。(ログの2行目、 INFOのところが空の`{}`になっています。)
![image](https://user-images.githubusercontent.com/44778704/101118298-1f81e700-362c-11eb-82d7-3ca8a8015447.png)

Lambdaプロキシ統合の使用に✅をつけます。
![image](https://user-images.githubusercontent.com/44778704/101118720-0168b680-362d-11eb-9020-d3cbd631e17c.png)


プロキシの統合設定をした後に実行すると、INFOに `event` の内容が表示されるようになりました

```
$ curl https://xxxxxxxx.execute-api.ap-northeast-1.amazonaws.com/dev/message?name=wakuwaku
```

![image](https://user-images.githubusercontent.com/44778704/101118613-cbc3cd80-362c-11eb-90b7-d4aeb59fec78.png)


ブラウザで実行すると、「Hero ◯◯(パラメータ)」と表示されるようになります。

APIはこれで公開できましたが、今の状態だと誰でも好き勝手にAPIを実行できてしまいます。
APIキーを設定して、キーを知っている人だけが実行できるようにするなどの工夫をした方がよい。
やり方は別の記事で後日書きます。

