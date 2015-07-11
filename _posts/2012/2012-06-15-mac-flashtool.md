---
layout: post
title: Mac で Flashtool を使ってダウングレード
date: '2012-06-15T19:49:54+09:00'
tags: []
---
Xperia mini (ST15i) の firmware を .62 にアップグレードした後、いわゆるバックキー連打病というのが悪化して、もうほとんど使い物にならなくなってしまった。回避するには root を取って修正のスクリプトを実行するしかないが、root を取るには .62 より前の firmware じゃないといけない。

ということでダウングレードする必要があるのだが、うちには Mac しかないし、検索するとみんな Windows でやっているみたいで(Mac の人は VMWare 上の Windows なんかでやっている)、Mac OSX でできないかいろいろ試したらできた(0.8.6)ので、手順を簡単に書いておく。

### 参考

- [Flashtool](http://androxyde.github.com/)
- [Mac で PaSoRi](http://mig-ration.blogspot.jp/2007/04/mac-pasori.html)
- [MacPorts](http://www.macports.org/)
- [LinuxでXperia X10を2.3に(FlashTool)](http://hanjuku-am2.blogspot.jp/2012/02/linuxxperia-x1023.html)
- [Xperia 2011 Easy Rooting Toolkit (xda developers)](http://forum.xda-developers.com/showthread.php?t=1320350)
- [Sony Ericsson Xperia mini(ST15i) 俺の基地](http://yakinikunotare.boo.jp/orebase2/sony_ericsson_xperia_mini_st15i)
- [ソニエリmini(S51SE) ホームキー連打病・解決編](http://blog.livedoor.jp/an_square/archives/51738525.html)

### 準備

#### ライブラリ (libusb)

libusb が必要だそうなのでそれをインストールする。MacPortsでインストールした。

1. MacPorts が入ってない場合はインストールして以下のコマンドで libusb をインストール
```sh
$ sudo port install libusb
```
2. 環境変数を設定
```sh
$ export JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.6.0/Home
$ export DYLD_FALLBACK_LIBRARY_PATH=/opt/local/lib
```

#### Android SDK

(Flashtool のアーカイブに adb.mac とか入ってるからいらないかもしれないが、root を取るのに必要なので入れておく。)

1. Android SDK をダウンロード
2. platform-tools をインストール
3. path を設定する
```sh
$ export PATH=$PATH:/path/to/android-sdk-linux/platform-tools
```

何度も実行することが考えられるなら、環境変数は .bash_profile とかに保存しておけばいいかな。

#### Flashtool

1. Flashtool と updates をダウンロード
2. Flashtool と updates を解凍し、updates のファイルで上書き

ここまでやってコマンドラインから Flashtool を実行すると (ダブルクリックでもいい) Flashtool が立ち上がる。あとは、いろいろなサイトに書いてあるように、Flash を選択して firmware を指定(ST15i_4.0.2.A.0.42_(1249-6227).ftf というのを使った)、電源を落とした Xperia のボリュームダウンボタンを押しながら USB 接続する。

#### root

1. Easy Rooting Toolkit と Rooting script for MAC OSX をダウンロード
2. Xperia で USB デバッグモードを有効にする (設定 > アプリケーション > 開発)
3. USB 接続
4. runme.sh を実行

#### 連打病の修正

1. Xperia に GScript Lite をインストール
2. 以下の内容でスクリプトファイルを作成し、SD の gscript フォルダに保存
```sh
dev=/sys/devices/platform/spi_qsd.0/spi0.0
fw=touch_smultron_sony.hex
cyttsp_fwloader -dev $dev -fw /system/etc/firmware/$fw -verify_only
sleep 5
reboot -p
```
3. GScript を起動して、保存したスクリプトを実行 (電源が落ちる)
4. 電源を入れる

最初に実行した時は、何故か起動後もまだバックキーが連打される状態だったけど、もう一度スクリプトを実行したら直った。それと、スクリプトを実行すると stderr と出るが、気にしなくてもいいみたい。そして、また発症したら、またスクリプトを走らす必要があるそうだ。。 ICS で治るといいが…。
