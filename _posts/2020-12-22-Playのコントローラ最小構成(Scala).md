---
layout: article
title:  Playのコントローラ最小構成(Scala)
tags: Scala Play
aside:
  toc: true
---

Playのプロジェクトを作成メモ。
Qiitaの記事を参考にすれば問題なし。

## 準備

### プロジェクト作成

[こちらの記事](https://qiita.com/yuichi0301/items/ead86d0251b954f07935)を参考にPlayプロジェクトを作成します。

```
mkdir test
cd test
sbt new playframework/play-java-seed.g8
```

しばらく待って・・

```
name [play-scala-seed]:
```
と出たらプロジェクト名(`play-hands-on`)を入力


#### 余談
Dockerで立てるならこちらの記事がGoodだった。
- [DockerでScala/sbt環境をお手軽に使う](https://qiita.com/yotsak/items/43cee726d44536208358)

### ルーティングの設定
`app/conf/routes`を編集
```
GET     /assets/*file               controllers.Assets.versioned(path="/public", file: Asset)
GET     /todo                       controllers.TodoController.findAll()
POST    /todo/register              controllers.TodoController.register()
```

`app/controller/TodoController`を作成
```scala
package controllers
package controllers
import play.api.libs.json._
import javax.inject._
import play.api.libs.json.JsValue
import play.api.mvc._

class TodoController @Inject()(mcc: MessagesControllerComponents)
  extends MessagesAbstractController(mcc) {

  // localhost:9000/todo
  def findAll() = Action { implicit request: MessagesRequest[AnyContent] =>
    Ok("get ok!")
  }

  // localhost:9000/todo/register
  def register() = Action(parse.json) { implicit request =>
    val param = request.body.toString()
    val json: JsValue = Json.parse(param)
    Ok("post ok! " + json)
  }
}



```

### 確認

Postmanで`localhost:9000/todo/register`にアクセス

![image](https://user-images.githubusercontent.com/44778704/102887128-b3abd500-4499-11eb-8777-a69620135ffb.png)


今回は以上。


