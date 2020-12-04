---
layout: article
title: AWS Lambda+DynamoDB+Gateway
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

子リソースも作成。この時、リソースパスの`id`は`{}`で囲むこと
![image](https://user-images.githubusercontent.com/44778704/100951085-28ea5100-3551-11eb-90d3-242e041ab220.png)



### メソッドの作成

usersを選択した状態でアクション>メソッドの作成を選択しGETメソッドを作る

![image](https://user-images.githubusercontent.com/44778704/100951177-60f19400-3551-11eb-9cde-92e1fde4f52f.png)


セットアップ画面が表示されるので先ほど作成したLambdaの情報と紐づける。

- 統合タイプ: Lambda関数
- Lambda関数: `get_users`

(Lambda関数(`get_users`)は入力候補を出してくれる・・・優秀すぎ・・・)
![image](https://user-images.githubusercontent.com/44778704/100847506-45d84300-34c3-11eb-8e19-252a839dee3f.png)

入力したら保存ボタンをクリック。さらに権限付与の警告が出てくるのでOKをクリック。
![image](https://user-images.githubusercontent.com/44778704/100848223-2d1c5d00-34c4-11eb-9921-dc79674f22ba.png)


完成したらこのようになる
![image](https://user-images.githubusercontent.com/44778704/100848316-4d4c1c00-34c4-11eb-8cfa-a178014b8cef.png)


APIのデプロイを選択
![image](https://user-images.githubusercontent.com/44778704/100951322-b29a1e80-3551-11eb-80db-9c8f1547a7f6.png)

- デプロイされるステージ: \[新しいステージ\]
- ステージ名: `prod`

![image](https://user-images.githubusercontent.com/44778704/100951399-d2314700-3551-11eb-81ec-2eb7a3051d79.png)


デプロイボタンを押したら完成！
URLをコピーし、`/users/1`をつけてブラウザで開いてみる
![image](https://user-images.githubusercontent.com/44778704/100959893-96eb4400-3562-11eb-8fa1-6b0c4f37ba95.png)



## エラー①

ランタイムエラー・・・
![image](https://user-images.githubusercontent.com/44778704/100960005-c7cb7900-3562-11eb-8342-628f695f144c.png)


```json
{
  "errorType":"TypeError",
  "errorMessage":"Cannot read property 'id' of undefined",
  "trace":["TypeError: Cannot read property 'id' of undefined","    at Runtime.exports.handler (/var/task/index.js:11:38)","    at Runtime.handleOnce (/var/runtime/Runtime.js:66:25)"]
}
```

コードを見直す




## エラー②

テストした際、DynamoDBのアクセス権限がないみたいなエラー
```
*** is not authorized to perform: dynamodb:GetItem on resource: ***
```

以下の記事をみながらロールを変えたら取得できるようになった！

[AWS lambdaからdynamodbに接続: "AccessDeniedException"で怒られたのでポリシーを設定した](https://qiita.com/hiroga/items/a672344efcf940e66485)



アクセス権限 > 編集ボタンをクリック
![image](https://user-images.githubusercontent.com/44778704/100969508-80e77e80-3576-11eb-9046-00d8eebd5199.png)


- AWSポリシーテンプレートから新しいロールを作成
- ポリシーテンプレートオプション
  - シンプルなマイクロサービスのアクセス権限を選択
![image](https://user-images.githubusercontent.com/44778704/100969608-aaa0a580-3576-11eb-9358-a8384ed3654c.png)

設定後にもう一度テストを実行したら成功した！

![image](https://user-images.githubusercontent.com/44778704/100969811-0d923c80-3577-11eb-8999-d9276ce9039b.png)

```json
{
  "statusCode": 200,
  "body": "{\"id\":\"1\",\"name\":\"Jill\"}"
}
```

以上

