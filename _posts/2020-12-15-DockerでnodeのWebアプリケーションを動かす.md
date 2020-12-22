---
layout: article
title: DockerでnodeのWebアプリケーションを動かす
tags: Docker Node.js
aside:
  toc: true
---

## Node.js アプリケーションを作成する

まずはNode.js公式の手順に沿ってコンテナを立ち上げてみる


### package.json作成
```json
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

パッケージをインストール
```
npm install
```

Dockerfile作成
```
vim Dockerfile
```

```
FROM node:12
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD [ "node", "server.js" ]
```

- `FROM node:12` = nodeをdockerhubからpullする
- WORKDIR = `cd /usr/src/app`する(ディレクトリがなければ作る)。その後のコマンドは全てこのディレクトリで実行したことになる。移動したければ再度WODKDIRと書けば良い。
- COPY package*.json ./ = ホスト側のカレントディレクトリにあるpackage.jsonとpackage.lock.jsonがコンテナ側にコピーされる
- RUN npm install = `npm install`が実行される。CMDは `docker run`の時に動くが、RUNはイメージ作成中に実行される
- COPY . . ホスト側のファイルをすべてコンテナ側にコピーする。ただし、Dockerignoで設定されているnode_modulesはコピーされない。
- EXPOSE 8080

### runする

```
docker run -p 49160:8080 -d node-app
```


### 動作確認

curlの場合

```
curl -i localhost:49160
```


