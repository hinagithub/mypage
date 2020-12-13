---
layout: article
title: dockerでMySQLを使う
tags: docker MySQL
aside:
  toc: true
---


お仕事でdockerの開発環境を作ることになったのでお勉強したメモ。
dockerhubのクイックリファレンスを読んで、まずはコマンドで試しつつ気になった点はメモしてみます。

## 参考
- [dockerhub -mysql-](https://hub.docker.com/_/mysql)
- [Docker mysql クイックリファレンス 日本語訳](https://qiita.com/JhonnyBravo/items/1643569510bc6fcd7189)

## コマンドを叩く

### イメージ取得

以下のコマンドを実行
```
docker pull mysql:5.7
```

`git images`実行結果
```
REPOSITORY                     TAG         IMAGE ID       CREATED         SIZE
mysql                          5.7         697daaecf703   18 hours ago    448MB
```

### コンテナ作成

以下のコマンドを実行

```
docker run -it --name mysql_ontainer_name -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 -d mysql:5.7
```

### オプション補足
- -it = stdinなどを可能にする
- --name = コンテナ名をつける
- -p = ポート番号を設定する(ホスト側:Docker側)
- -e = 環境変数を設定する
  - MYSQL_ROOT_PASSWORD = rootのパスワードを設定(必須)
  - MYSQL_DATABASE = DBを作成 (指定しなくてもデフォルトDBが作られる)
  - MYSQL_USER, MYSQL_PASSWORD = ユーザとパスワードを設定。MYSQL_USER, MYSQL_PASSWORDで設定したDBに対してスーパーユーザと同じ権限を持つ。
  - MYSQL_ALLOW_EMPTY_PASSWORD = パスワードを空欄にできる
  - MYSQL_RANDOM_ROOT_PASSWORD = rootパスワードはランダムに設定され、その文字列はコンソールに表示される。
  - MYSQL_ONETIME_PASSWORD = 一時的なユーザを作成でき、初回ログイン時にPWリセットを求められる

### オプション使用例
- MYSQL_RANDOM_ROOT_PASSWORD
```
docker run -it --name test-mysql -e docker run -it --name test-mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true -d mysql:5.7
```

生成されたパスワードは`docker logs container名`で確認可能。
↓logs実行結果の一部
```
2020-12-12 12:00:01+00:00 [Note] [Entrypoint]: GENERATED ROOT PASSWORD: axxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx5
```

### 初期データを取り込む
`/docker-entrypoint-initdb.d`にある`.sql`ファイルは自動的に初期値として取り込まれる。
<br />
なので、`docker run`する際に`-v`でホスト側に用意した`.sql`ファイルをそのディレクトリに格納すればよい。
<br />
参考:[サンプルデータがあらかじめ入った MySQL を Docker で作成する](https://www.xlsoft.com/jp/blog/blog/2019/10/09/post-7617/)

`data.sql`
```sql
DROP SCHEMA IF EXISTS sample;
CREATE SCHEMA sample;
USE sample;

DROP TABLE IF EXISTS employee;

CREATE TABLE employee
(
  id           INT(10),
  name     VARCHAR(40)
);

INSERT INTO employee (id, name) VALUES (1, "Nagaoka");
INSERT INTO employee (id, name) VALUES (2, "Tanaka");
```

data.sqlをマウントする

```
docker run -v /Users/hinako/docker/work/sql:/docker-entrypoint-initdb.d -it --name test-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.7
```

コンテナに入ってファイルがマウントされていることを確認
```
docker exec -it コンテナID /bin/bash
cat docker-entrypoint-initdb.d/data.sql
```

コンテナから出る
```
exit
```

mysqlを操作してデータが作られているかを確認
```
docker exec -it CONTAINER_NAME mysql -u root -p
```

パスワードを入力したらSQLコマンドでDBを確認

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sample             |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

```

つづけてテーブルを確認

```
mysql> use sample
Database changed

mysql> show tables;
+------------------+
| Tables_in_sample |
+------------------+
| employee         |
+------------------+
1 row in set (0.00 sec)

```

続けてSELECTしてみる
```
mysql> select * from employee;
+------+---------+
| id   | name    |
+------+---------+
|    1 | Nagaoka |
|    2 | Tanaka  |
+------+---------+
2 rows in set (0.00 sec)

```
できてます！

### docker-composeにする

コマンドではなくdocker-composeを使いたいので設定します。

<br />
<br />
その前に今まで作ったコンテナは一旦削除しておく。


```
docker ps -a
```

実行結果
```
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                    NAMES
f1884bfed14f   mysql:5.7            "docker-entrypoint.s…"   28 minutes ago   Up 28 minutes   3306/tcp, 33060/tcp      work_db_1
```

コンテナIDを指定して削除
```
docker rm -f f1
```

docker-compose.ymlを作成
```yaml
version: '3.1'
services:
  db:
    image: mysql:5.7
    volumes:
      - ./sql:/docker-entrypoint-initdb.d
    environment:
      - MYSQL_ROOT_PASSWORD=my-secret-pw
```

実行
```
docker-compose up -d
```

あとはコマンドで叩いた時と同じ手順で初期データがちゃんと登録されているかを確認すればOK。


### その他(注意)
- docker-composeでMySQLの並行稼働を行うとinitializeでこける可能性あり。connection-loopなどの工夫が必要になるかも。
- dumpデータをリストアしたいときは`exec -i`を使うなどの方法もある




