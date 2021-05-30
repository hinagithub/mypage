---
layout: article
title: MulterでファイルとJSONボディの両方のリクエストを受け取る方法
tags: Node.js Express ファイル操作
aside:
  toc: true
---

## 概要

Node.js + Expressで開発中、ファイルとJSONのリクエストをうけるコードを書いた。やり方にちょっとハマったのでメモ。

<br/>
<br/>

参考:
- [Uploading form fields and files at the same time with Multer (Node.js, Typescript)](https://medium.com/developer-rants/uploading-form-fields-and-files-at-the-same-time-with-multer-node-js-typescript-c1a367eb8198)
- [package -Multer-](https://www.npmjs.com/package/multer)
- [Multerでファイルをアップロードする]()

## ソースコード

「[Multerでファイルをアップロードする]()」を参考にプロジェクトを作成。
そしてapp.jsに以下のupload_multiというAPIを追加する。


```js
app.post('/upload_multi', upload.any(), (req, res) => {
  // ファイルの中身
  console.log(req.files);
  const bookCsvFile = req.files.find((file) => file.fieldname === 'book_csv');
  const foodCsvFile = req.files.find((file) => file.fieldname === 'food_csv');
  console.log('book file : ', bookCsvFile);
  console.log('food file : ', foodCsvFile);

  // JSONの中身
  console.log('body : ', req.body);
  console.log('body.data : ', req.body.data);

  res.send(
    `file : ${JSON.stringify(req.files)}, body : ${JSON.stringify(req.body)}`
  );
});
```

## リクエスト情報

curlコマンドで試す。
```python
curl -v 'http://localhost:3000/upload_multi' -XPOST \
-F book_csv=@'/Users/hinako/node/csv/tmp/test_books.csv' \
-F food_csv=@'/Users/hinako/node/csv/tmp/test_foods.csv' \
-F data='{"id":1,"name":"taro"}'
```

## 解説

### upload.any()

```js
app.post('/upload_multi', upload.any(), (req, res) => {
```
`upload.any()`がポイント。
ファイルを1つ受けとるだけなら`upload.single(フィールド名)`でOKだが、今回のように複数のファイルを受け取る時はany()を使う。

ちなみにMulterのオプションはこちら
[]()

#### .single(fieldname)
ファイルを一つだけ受け取る。`req.file.`で受け取ったファイルの情報を取得可能。

#### .array(fieldname[, maxCount])
複数のファイルを受け取れる。 `maxCount`で受け取るファイル数の上限数を指定可能。`req.files.`で受け取ったファイルの情報を取得可能。

#### .fields(fields)
複数ファイルを受け取れる。`req.files.`で受け取ったファイルの情報を取得可能。
.arrayはフィールド名がそれぞれ異なる複数のファイルを受け取るが、.fieldsは同じフィールド名で複数のファイルを受け取る。多分。

#### .none()
テキストのみ受け取る。もしファイルがアップロードされたら、`"LIMIT_UNEXPECTED_FILE"`というエラーになる。

#### .any()
全てのファイルを受け取る。`req.files.`で受け取ったファイルの情報を取得可能。


#### .any()に関する注意
なんでもかんでも受け取るので、悪意のあるユーザから変なファイルを受け取ってしまうことも。なのでグローバルミドルウェアには設定しないこと。リクエストされたファイルは必ずハンドリングしましょう。

グローバルミドルウェアはおそらく、routesの呼び出し前に app.useで設定しているような、必ず呼ばれるミドルウェアのこと。authチェックなどでよく設定している。

- グローバルミドルウェアの例1
```
app.use(function(req, res, next){
  if(req.isAuthenticated()) next();
  else res.send(false);
});

// routing code
app.get('/', function(req, res){});
app.get('/anything', function(req,res){})
//...
```

- グローバルミドルウェアの例2
```
function ensureUser(req,res,next){
   if(req.isAuthenticated()) next();
   else res.send(false);
}
app.get('/anything', ensureUser, function(req,res){
    // some code
})
```

### req.files
```js
  console.log(req.files);
```

ここで出力した中身は以下のようになっている
```js
[
  {
    fieldname: 'book_csv',
    originalname: 'test_books.csv',
    encoding: '7bit',
    mimetype: 'application/octet-stream',
    destination: '/Users/hinako/node/csv/tmp/',
    filename: '1622360658829-217471662-book_csv-test_books.csv',
    path: '/Users/hinako/node/csv/tmp/1622360658829-217471662-book_csv-test_books.csv',
    size: 70
  },
  {
    fieldname: 'food_csv',
    originalname: 'test_foods.csv',
    encoding: '7bit',
    mimetype: 'application/octet-stream',
    destination: '/Users/hinako/node/csv/tmp/',
    filename: '1622360658832-184418623-food_csv-test_foods.csv',
    path: '/Users/hinako/node/csv/tmp/1622360658832-184418623-food_csv-test_foods.csv',
    size: 46
  }
]
```

各ファイルはフィールド名(`files[番号].fieldname`)で識別が可能。
```js
  const bookCsvFile = req.files.find((file) => file.fieldname === 'book_csv');
  const foodCsvFile = req.files.find((file) => file.fieldname === 'food_csv');
  console.log('book file : ', bookCsvFile);
  console.log('food file : ', foodCsvFile);
```
出力結果は以下のようになっている
```js
book file :  {
  fieldname: 'book_csv',
  originalname: 'test_books.csv',
  encoding: '7bit',
  mimetype: 'application/octet-stream',
  destination: '/Users/hinako/node/csv/tmp/',
  filename: '1622360658829-217471662-book_csv-test_books.csv',
  path: '/Users/hinako/node/csv/tmp/1622360658829-217471662-book_csv-test_books.csv',
  size: 70
}
food file :  {
  fieldname: 'food_csv',
  originalname: 'test_foods.csv',
  encoding: '7bit',
  mimetype: 'application/octet-stream',
  destination: '/Users/hinako/node/csv/tmp/',
  filename: '1622360658832-184418623-food_csv-test_foods.csv',
  path: '/Users/hinako/node/csv/tmp/1622360658832-184418623-food_csv-test_foods.csv',
  size: 46
}
```

(各項目の説明は、名前を見れば明らかなので割愛)

## req.body
body情報はExpressの標準仕様通り、req.bodyで受け取れる。
```js
console.log('body : ', req.body);
```

出力結果は以下。
```js
body :  [Object: null prototype] { data: '{"id":1,"name":"taro"}' }
body.data :  {"id":1,"name":"taro"}
```

きちんとJSONで受け取れている。

</br>
</br>

以上。
