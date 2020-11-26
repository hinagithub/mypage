---
layout: article
title: SDKMANとJDK
tags: JVM
aside:
  toc: true
---

ロゴがアメコミぽくてかあいい☺️
そんなSDKMANを使ってみます
![image](https://user-images.githubusercontent.com/44778704/100208717-6137d100-2f4c-11eb-91dc-8c9234138352.png)

JVM系の言語やツールを管理するのに便利らしいです。

さっそくインストール。
Ubuntuだとまずはunzipなどが必要なのでそちらも。

[How to Install SDKMAN on Ubuntu](https://blog.romach007.com/how-to-install-sdkman-on-ubuntu)

```
sudo apt-get install unzip
sudo apt-get install zip
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

![image](https://user-images.githubusercontent.com/44778704/100209305-0ce12100-2f4d-11eb-9d83-abb94b725325.png)


・・・かあいい☺️


インストールできたら`sdk`コマンドが使えるようになります。
やってみましょう。

```
sdk list java
```

実行結果

```
vagrant@vagrant:~/scala_lessions$ sdk list java
==== BROADCAST =================================================================
* 2020-11-24: jbang 0.54.1 available on SDKMAN! https://git.io/JkX8F
* 2020-11-23: kotlin 1.4.20 available on SDKMAN!
* 2020-11-22: jbang 0.54.0 available on SDKMAN! https://git.io/Jkw8M
================================================================================
================================================================================
Available Java Versions
================================================================================
 Vendor        | Use | Version      | Dist    | Status     | Identifier
--------------------------------------------------------------------------------
 AdoptOpenJDK  |     | 15.0.1.j9    | adpt    |            | 15.0.1.j9-adpt
               |     | 15.0.1.hs    | adpt    |            | 15.0.1.hs-adpt
               |     | 14.0.2.j9    | adpt    |            | 14.0.2.j9-adpt
               |     | 14.0.2.hs    | adpt    |            | 14.0.2.hs-adpt
               |     | 13.0.2.j9    | adpt    |            | 13.0.2.j9-adpt
               |     | 13.0.2.hs    | adpt    |            | 13.0.2.hs-adpt
               |     | 12.0.2.j9    | adpt    |            | 12.0.2.j9-adpt
               |     | 12.0.2.hs    | adpt    |            | 12.0.2.hs-adpt
               |     | 11.0.9.open  | adpt    |            | 11.0.9.open-adpt
               |     | 11.0.9.j9    | adpt    |            | 11.0.9.j9-adpt
               |     | 11.0.9.hs    | adpt    |            | 11.0.9.hs-adpt
               |     | 8.0.275.open | adpt    |            | 8.0.275.open-adpt
               |     | 8.0.275.j9   | adpt    |            | 8.0.275.j9-adpt
               |     | 8.0.275.hs   | adpt    |            | 8.0.275.hs-adpt
               |     | 8.0.272.hs   | adpt    |            | 8.0.272.hs-adpt
 Alibaba       |     | 11.0.8       | albba   |            | 11.0.8-albba
               |     | 8u272        | albba   |            | 8u272-albba
 Amazon        |     | 15.0.1       | amzn    |            | 15.0.1-amzn
               |     | 11.0.9       | amzn    |            | 11.0.9-amzn
               |     | 8.0.275      | amzn    |            | 8.0.275-amzn
 Azul Zulu     |     | 15.0.1       | zulu    |            | 15.0.1-zulu
               |     | 15.0.1.fx    | zulu    |            | 15.0.1.fx-zulu
               |     | 14.0.2       | zulu    |            | 14.0.2-zulu
               |     | 14.0.2.fx    | zulu    |            | 14.0.2.fx-zulu
               |     | 13.0.5       | zulu    |            | 13.0.5-zulu
               |     | 13.0.5.fx    | zulu    |            | 13.0.5.fx-zulu
               |     | 12.0.2       | zulu    |            | 12.0.2-zulu
               |     | 11.0.9       | zulu    |            | 11.0.9-zulu
               |     | 11.0.9.fx    | zulu    |            | 11.0.9.fx-zulu
               |     | 10.0.2       | zulu    |            | 10.0.2-zulu
               |     | 9.0.7        | zulu    |            | 9.0.7-zulu
               |     | 8.0.272      | zulu    |            | 8.0.272-zulu
               |     | 8.0.272.fx   | zulu    |            | 8.0.272.fx-zulu
               |     | 7.0.282      | zulu    |            | 7.0.282-zulu
               |     | 6.0.119      | zulu    |            | 6.0.119-zulu
 BellSoft      |     | 15.0.1.fx    | librca  |            | 15.0.1.fx-librca
               |     | 15.0.1       | librca  |            | 15.0.1-librca
               |     | 14.0.2.fx    | librca  |            | 14.0.2.fx-librca
               |     | 14.0.2       | librca  |            | 14.0.2-librca
               |     | 13.0.2.fx    | librca  |            | 13.0.2.fx-librca
               |     | 13.0.2       | librca  |            | 13.0.2-librca
               |     | 12.0.2       | librca  |            | 12.0.2-librca
               |     | 11.0.9.fx    | librca  |            | 11.0.9.fx-librca
               |     | 11.0.9       | librca  |            | 11.0.9-librca
               |     | 8.0.275.fx   | librca  |            | 8.0.275.fx-librca
               |     | 8.0.275      | librca  |            | 8.0.275-librca
               |     | 8.0.265.fx   | librca  |            | 8.0.265.fx-librca
 GraalVM       |     | 20.3.0.r11   | grl     |            | 20.3.0.r11-grl
               |     | 20.3.0.r8    | grl     |            | 20.3.0.r8-grl
               |     | 20.2.0.r11   | grl     |            | 20.2.0.r11-grl
               |     | 20.2.0.r8    | grl     |            | 20.2.0.r8-grl
               |     | 20.1.0.r11   | grl     |            | 20.1.0.r11-grl
               |     | 20.1.0.r8    | grl     |            | 20.1.0.r8-grl
               |     | 20.0.0.r11   | grl     |            | 20.0.0.r11-grl
               |     | 20.0.0.r8    | grl     |            | 20.0.0.r8-grl
               |     | 19.3.4.r11   | grl     |            | 19.3.4.r11-grl
               |     | 19.3.4.r8    | grl     |            | 19.3.4.r8-grl
               |     | 19.3.1.r11   | grl     |            | 19.3.1.r11-grl
               |     | 19.3.1.r8    | grl     |            | 19.3.1.r8-grl
 Java.net      |     | 16.ea.25     | open    |            | 16.ea.25-open
               |     | 16.ea.7.lm   | open    |            | 16.ea.7.lm-open
               |     | 16.ea.2.pma  | open    |            | 16.ea.2.pma-open
               |     | 15.0.1       | open    |            | 15.0.1-open
               |     | 14.0.2       | open    |            | 14.0.2-open
               |     | 13.0.2       | open    |            | 13.0.2-open
               |     | 12.0.2       | open    |            | 12.0.2-open
               |     | 11.0.2       | open    |            | 11.0.2-open
               |     | 10.0.2       | open    |            | 10.0.2-open
               |     | 9.0.4        | open    |            | 9.0.4-open
               |     | 8.0.265      | open    |            | 8.0.265-open
 Mandrel       |     | 20.2.0.0     | mandrel |            | 20.2.0.0-mandrel
 SAP           |     | 15.0.1       | sapmchn |            | 15.0.1-sapmchn
               |     | 14.0.2       | sapmchn |            | 14.0.2-sapmchn
               |     | 13.0.2       | sapmchn |            | 13.0.2-sapmchn
               |     | 12.0.2       | sapmchn |            | 12.0.2-sapmchn
               |     | 11.0.9       | sapmchn |            | 11.0.9-sapmchn
 TravaOpenJDK  |     | 11.0.9       | trava   |            | 11.0.9-trava
               |     | 8.0.232      | trava   |            | 8.0.232-trava
================================================================================
Use the Identifier for installation:

    $ sdk install java 11.0.3.hs-adpt
================================================================================

```

Javaってたくさんバージョンあるみたい。

JDKについてちょっとおさらい


#### JDKとは
Java Development Kitの略。
**Javaプログラムを作るとき**に必要な要素を詰め込んだもの。
Java開発キットともいう。


#### JREとは
Java Runtime Environmentの略。
**Javaプログラムを動かすとき**に必要な要素を詰め込んだもの。
Java実行環境ともいう。


#### JDKのディストリビューション
JDKにも色々あって、いろんな団体や企業がそれぞれ使いやすくしたものを提供している感じ。
Linuxのディストリビューションと同じ感覚でOKだと思う。

#### AdoptOpenJDK
AdoptOpenJDコミュニティが提供しているOpenJDKディストリビューション（配布形態）。

#### Alibaba(Alibaba Dragonwell)
あの中国のECサイトのアリババのことだと思う。
OracleJDK8のサポートが2020年8月で切れるからオープンソースにしたという記事もみつけた。
[avaコミュニティを盛り上げる：アリババがDragonwell OpenJDKをオープンソース化](https://qiita.com/KentOhwada_AlibabaCloudJapan/items/44d0882bbaf4cd24d009)

#### Amazon(Amazon Corretto)
Amazonが出しているのも知らなかった。手広い。
[Amazon Corretto](https://aws.amazon.com/jp/corretto/)

#### Azul Zulu(Zulu/Zulu)
Azul Systemsという会社が提供している。
一瞬microsoftのArureかと思ったけど違いました。

#### BellSoft(Liberica JDK)
初めて聞きました。
あんまり有名じゃないのかな。
[BellSoft](https://bell-sw.com/)

#### GraalVM
ユニバーサルなVMなのでJava意外にもJavaScriptやPythonも実行できるらしいです。
[GraalVMに入門する](https://tech.uzabase.com/entry/2020/01/27/090000)


#### Java.net
オラクルが出してますがOracleJDKとはまた違うのかな・・？
[jdk.java.net logo](https://jdk.java.net/)
[JDK、Oracle JDK、OpenJDK、Java SEってなに？](https://qiita.com/nowokay/items/c1de127354cd1b0ddc5e)



#### Mandrel(Red Hat・GraalVMコミュニティ)
Red HatとGraalVMコミュニティが共同開発したもの。
KubernetesネイティブなJavaフレームワーク`Quarkus`のために作られたらしい

補足: GraalVMとは
> GraalVMは、Oracleがオープンソースとして2018年4月に公開した、Java・JavaScript・Ruby・Pythonなどの多言語を、統合された一つのランタイム上で実行させる汎用ランタイムです。

補足: Quarkusとは
> Java開発者がこれまで培ってきた知識・スキルを活かしつつ、KubernetesネイティブなコンテナアプリケーションをJavaを使ってデプロイするため登場したのが「Quarkus」です。

参考
[KubernetesネイティブなJavaフレームワーク Quarkus について調査してみた件](https://qiita.com/mamomamo/items/e73e5216605609e40082)


#### SAP
引用
>ざっくり言えばSAPがSAP関連のサーバサイドソフトウェアを開発・運用・サポートしやすいようにOracleのHotspot VMとJDKから修正したものです。

[SAP JVMについてOracle JDKとの違いを調べてみた(jvmmonという便利ツールは知っておきたい)](https://yomon.hatenablog.com/entry/2018/02/20/104528)

#### TravaOpenJDK
有名じゃなさそう。ググってもあんまり情報出てこない。。


以上、



## 全体の参考
[Javaのサポートについてのまとめ2018](https://qiita.com/nowokay/items/edb5c5df4dbfc4a99ffb)
