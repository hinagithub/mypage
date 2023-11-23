---
layout: article
title: Raspberry Piのセットアップ
tags: RaspberryPi
aside:
  toc: true
---


Raspberry Pi4 ModelBを買ったので早速組み立てゆく


# 用意するもの

買ったのは以下

- [Vemico Raspberry Pi 4 Model B(RAM 4GB)/ラズベリーパイ4b/技適マーク付き/ 32GBのMicroSDカード/ 5V 3A ON/OFFスイッチ付き電源/MicroHDMI-to-HDMIケーブルライン/CAT6ネットケーブル/ラズパイ専用ケース/冷却ファン/三つヒートシンク/ドライバー/カードリーダ 日本語取扱説明書 付](https://www.amazon.co.jp/gp/product/B07ZRF5BC1/ref=ppx_yo_dt_b_asin_image_o07_s00?ie=UTF8&psc=1)

<img width="300" alt="image" src="https://github.com/hinagithub/mypage/assets/44778704/ae82c1cb-51e4-4b6d-9465-764bceec0e53">


- [ラズベリーパイ アクリルケース](https://www.amazon.co.jp/gp/product/B094XM1GQM/ref=ppx_yo_dt_b_asin_image_o00_s00?ie=UTF8&psc=1)

<img width="300" alt="image" src="https://github.com/hinagithub/mypage/assets/44778704/1a148287-09e0-4640-bdc0-cfe7e3f8ddac">

- [SunFounder Raspberry Pi 用のセンサーキット,37 IN 1(37モジュール入り)](https://www.amazon.co.jp/gp/product/B089LZPP3D/ref=ppx_yo_dt_b_asin_image_o07_s00?ie=UTF8&psc=1)

<img width="300" alt="image" src="https://github.com/hinagithub/mypage/assets/44778704/f50c8704-661d-42e0-bb06-d72901236978">


- 有線キーボード

あとは

- マウス
- モニタ
- MacOS用SDカードリーダ

も必要だけど家にあったもので間に合った

本体ケースは付属の黒いものだとリボンケーブルが上手くさせないので、横に広い隙間のある別のケースを購入した

#　組み立て

説明書が入っているのでその通りに仕上げていく
わかりずらいところはYoutubeの動画で補う

組み立た仕上がりはこんな感じ

<img width="300" alt="image" src="https://github.com/hinagithub/mypage/assets/44778704/695e7761-25c4-49ad-a3f6-5961779ef1df">

CUPコア等の上に貼るシールはヒートシンクと言うらしい

>放熱シート／熱伝導シート(ヒートシンク)とは電子機器類等に実装される発熱部品に密着させて設置することにより、熱を効率的に吸熱し、発熱部位から離れた場所へ放熱することで機器類の誤作動や故障を防止する。

## OSセットアップ

手順は
1. MacでラズパイOSをダウンロード
2. SDカードに移す
3. ラズパイにSDカードを刺す

という流れ。

まずは公式サイトから持ってくる。
NOOBS(New Out Of the Box Software)を使う方法も紹介しているかこちらは使わない。
これはただのインストーラーマネージャであり、これを使わなくてもインストールは簡単にできる。

公式サイトからOSをダウンロード
https://www.raspberrypi.com/software/

プログラムを開いてインストールの設定をする。

<img width="300" alt="image" src="https://github.com/hinagithub/mypage/assets/44778704/efc8c2bf-4511-4371-9e12-9f9cebfdf496">


- Raspberrypiデバイス: 4 ModelB
- OS 64Bitにする(32でもまあいいけど)
- ストレージ: MacにSDカードを挿せば自動で認識してくれるはず

MacにはSDカードを読むクチがないのでTypeCで読めるような外部接続のSDカードリーダがいる

↓こんな感じ

<img width="200" alt="image" src="https://github.com/hinagithub/mypage/assets/44778704/5e136b15-303a-4838-9afa-e1039203bb7f">

SDカードは大きいのと小さいのが付属されているが小さい方(microSD)を使う

ラズパイで使えるカードにはスピードや容量など制約があるよう。

とりあえずSunDiskを使っておけば間違いなさそう。

(逆にこれ以外だと規格に乗っ取っていても動かなかったと言う話を聞いたことがあるので自分はこれ一択)

<img width="200" alt="image" src="https://github.com/hinagithub/mypage/assets/44778704/9b4e904e-fa2e-4c97-86e6-fea359a19fe9">


OSをSDカードに書き込み終わったらリーダから取り出してラズパイに刺す。

起動すると言語設定聞かれるのでJapaneseにしておく。
ユーザ名とパスワードも適当に入れる。

そこからはセットアップのためにしばらく待つ。

お風呂入って出てきた頃には完了している。


# 少し触ってみる
ターミナルを起動してホームに移動してみる
```
> cd ~/
> pwd

/home/hinaberry
```
ちゃんとLinuxだった。

次に必要なものをインストールする。

- Golang
- Git
- Vim

```
sudo apt update
sudo apt install git
sudo apt install vim
```

ここで初めて知ったんだけど最近は

`apt-get ...` ではなく

`apt ...` と書けるらしい。

直感的にわかっていいね！

Golang導入は一癖あったのでまた別の記事に書く。

以上。
