---
layout: article
title: 【Vue】firebaseStorage
tags: Vue.js Firebase
aside:
  toc: true
---

# Vueでfirebase storageを使う


## 参照の作成

```js

//method
getRef() {
      const storageRef = firebase.storage().ref("images/" + this.file.name);

      console.log(storageRef.fullPath)
      // images/1538204593.png
      console.log(storageRef.name)
      // Home.vue?76f2:348 1538204593.png
      console.log(storageRef.bucket)
      // Home.vue?🤫 pj004-vue-slack-clone.appspot.com
    },

```

## ファイルのアップロード

アップロードだけなら`const uploadTask = storageRef.put(this.file);`でOK</br>
アップロードの進捗を追うなら以下のメソッドがいい

- running:開始
- progress:進捗
- pause:一時停止

snapshotには以下のプロパティがあるのでそれらをもとにゴニョゴニョできる

- bytesTransferred: 作成された時点のバイト数
- totalBytes: アップロードされる予定のバイト数
- state: アップロードの現在の状態

などなど

```js

fileUpload() {
      const storageRef = firebase.storage().ref("images/" + this.file.name);
      const uploadTask = storageRef.put(this.file);

      // アップロードの進捗をモニタリング(progressを使用)
      uploadTask.on(
        "state_changed",
        snapshot => {
          const percentage =
            (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
          this.$refs.progress_bar.style.width = percentage + "%";
        },
        error => {
          console.log(error);
        },
        () => {
          storageRef.getDownloadURL().then(url => {
            this.url = url;
            this.sendMessage();
            this.file_upload_modal = false;
          });
        }
      );
    },


```
