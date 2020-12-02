---
layout: article
title: AWS LambdaでAPI作成
tags: Scala
aside:
  toc: true
---

こちらの記事を参考にAWS LambdaでサーバレスAPIを作ってみます
[初心者が５分で出来る簡単サーバレスAPIを構築してみる【Lambda】](https://recipe.kc-cloud.jp/archives/16877)



## DynamoDBの設定


### テーブル作成

- テーブル名: test
- プライマリキー: id
- デフォルト設定の使用: ON

![image](https://user-images.githubusercontent.com/44778704/100842692-926c5000-34bc-11eb-919a-aa9cd2fb1ff7.png)



### テーブル完成を確認

![image](https://user-images.githubusercontent.com/44778704/100843059-2211fe80-34bd-11eb-9842-9a7436b5f550.png)


### 項目の作成
![image](https://user-images.githubusercontent.com/44778704/100843367-99479280-34bd-11eb-93a7-b91b453357f2.png)

### フィールドの追加
＋ボタンを押してフィールドを追加します。
![image](https://user-images.githubusercontent.com/44778704/100843506-ceec7b80-34bd-11eb-8cf7-f41647f373a0.png)

3つくらい作っておきます。
![image](https://user-images.githubusercontent.com/44778704/100843714-1d017f00-34be-11eb-8b20-67e913797c26.png)


## Lambdaの設定

### Lambda関数を作成

- 関数名: get_user
- ランタイム: Node.js 12.x
- デフォルトの実行ロールの変更
  - 基本的な Lambda アクセス権限で新しいロールを作成: ON
![image](https://user-images.githubusercontent.com/44778704/100843951-7669ae00-34be-11eb-98f2-e001f92d16ab.png)


「関数の作成」ボタンを押下後少し待ち、以下の画面になれば作成完了!
![image](https://user-images.githubusercontent.com/44778704/100844234-ec6e1500-34be-11eb-86ee-5760aac0fea2.png)

画面の下の方にindex.jsを入力するところがあるので以下のコードをコピペし右上の`デプロイ`ボタンをクリック

```js
 'use strict';
　const AWS = require('aws-sdk');
　const dynamo = new AWS.DynamoDB.DocumentClient();
　const createResponse = (statusCode, body) => ({ statusCode, body });
　
　exports.handler = (event, context, callback) => {
  // テーブル名を指定
    let params = {
        TableName: 'test' ,
        Key: {
            id: event.pathParameters.id
        }
    };

    let dbGet = (params) => { return dynamo.get(params).promise() };

    dbGet(params).then( (data) => {
        if (!data.Item) {
            callback(null,createResponse(404, "ITEM NOT FOUND"));
            return;
        }
        callback(null, createResponse(200, JSON.stringify(data.Item)));
    }).catch( (err) => {
        callback(null, createResponse(500, err));
    });
};
```


## API Gatewayの設定

### APIのタイプを選択
REST API(プライベートじゃない方)のインポートボタンをクリック
![image](https://user-images.githubusercontent.com/44778704/100845920-3e179f00-34c1-11eb-80eb-fcc58af3e3ed.png)

新しいAPIの作成画面が出てくるので右下の「作成」(?)ボタンをクリック。
(ここのキャプチャ撮り忘れました)

### テンプレートのリソースを削除
リソース画面が表示されるので、アクションの「APIの削除」を選択し、`/`配下のメソッドを消していったんまっさらにする
![image](https://user-images.githubusercontent.com/44778704/100846695-2b519a00-34c2-11eb-84e1-379f38951769.png)


メソッドを消してまっさらな状態にすると以下の画面ようになる

![image](https://user-images.githubusercontent.com/44778704/100846945-7a97ca80-34c2-11eb-9d26-94a3d2f8bf74.png)

### リソースの作成
アクション > リソースの作成から `users` リソースを作成する
![image](https://user-images.githubusercontent.com/44778704/100847095-adda5980-34c2-11eb-8fbe-3b35e3148bf7.png)

usersリソースが完成
![image](https://user-images.githubusercontent.com/44778704/100847234-e37f4280-34c2-11eb-96f9-47295ea43809.png)

### メソッドの作成

usersを選択した状態でアクション>メソッドの作成を選択しGETメソッドを作る

![image](https://user-images.githubusercontent.com/44778704/100847314-027dd480-34c3-11eb-924b-2b709712a312.png)

セットアップ画面が表示されるので先ほど作成したLambdaの情報と紐づける。

- 統合タイプ: Lambda関数
- Lambda関数: `get_users`

(Lambda関数(`get_users`)は入力候補を出してくれる・・・優秀すぎ・・・)
![image](https://user-images.githubusercontent.com/44778704/100847506-45d84300-34c3-11eb-8e19-252a839dee3f.png)

入力したら保存ボタンをクリック。さらに権限付与の警告が出てくるのでOKをクリック。
![image](https://user-images.githubusercontent.com/44778704/100848223-2d1c5d00-34c4-11eb-9921-dc79674f22ba.png)


完成したらこのようになる
![image](https://user-images.githubusercontent.com/44778704/100848316-4d4c1c00-34c4-11eb-8cfa-a178014b8cef.png)



・・・続きは作成中

