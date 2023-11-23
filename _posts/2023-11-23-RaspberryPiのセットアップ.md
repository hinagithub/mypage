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

<img width="442" alt="image" src="https://github.com/hinagithub/secret/assets/44778704/16c41744-2020-4592-96b5-7660d7d0d3c6">

- [ラズベリーパイ アクリルケース](https://www.amazon.co.jp/gp/product/B094XM1GQM/ref=ppx_yo_dt_b_asin_image_o00_s00?ie=UTF8&psc=1)

<img width="704" alt="image" src="https://github.com/hinagithub/secret/assets/44778704/eb39ed45-2ac8-48a7-949f-ed5d14d8bc6a">

- [SunFounder Raspberry Pi 用のセンサーキット,37 IN 1(37モジュール入り)](https://www.amazon.co.jp/gp/product/B089LZPP3D/ref=ppx_yo_dt_b_asin_image_o07_s00?ie=UTF8&psc=1)

<img width="694" alt="image" src="https://github.com/hinagithub/secret/assets/44778704/c327ff2e-303a-44af-a595-62ff8fbc3b49">


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

<img width="1203" alt="image" src="https://github.com/hinagithub/secret/assets/44778704/b80a7da3-a762-41de-b044-bc848cd56103">

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

<img width="973" alt="image" src="https://github.com/hinagithub/secret/assets/44778704/0b8f12cd-725b-4564-8709-5c878061d64f">

- Raspberrypiデバイス: 4 ModelB
- OS 64Bitにする(32でもまあいいけど)
- ストレージ: MacにSDカードを挿せば自動で認識してくれるはず

MacにはSDカードを読むクチがないのでTypeCで読めるような外部接続のSDカードリーダがいる

↓こんな感じ

<img width="398" alt="image" src="https://github.com/hinagithub/secret/assets/44778704/e35f4683-7b2d-4bbe-8345-b54e1d31778f">

SDカードは大きいのと小さいのが付属されているが小さい方(microSD)を使う

ラズパイで使えるカードにはスピードや容量など制約があるよう。

とりあえずSunDiskを使っておけば間違いなさそう。

(逆にこれ以外だと規格に乗っ取っていても動かなかったと言う話を聞いたことがあるので自分はこれ一択)

<img width="264" alt="image" src="https://github.com/hinagithub/secret/assets/44778704/f955ae31-69f4-42bc-826e-2f686e5cf935">


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
