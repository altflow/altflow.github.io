---
layout: post
title: Jailbreak (iPod touch 4G, iOS 4.3.2)
date: '2011-05-02T22:01:50+09:00'
tags: []
---
ふと思い立って Jailbreak してみた。Pwnage Tool の Simple Mode で
表示される指示に従ったらあっけなくできた(ちなみに検索すると Expert Mode で
.ipsw ファイルを自分で指定する方法を良く見かけるが、ipsw ファイルは
`/Users/Username/Library/iTunes/iPod Software Updates` にある)。

Jailbreak した後、使っているうちに落ちて、林檎マーク -> パスコード入力画面が
ループして焦った。何度か再起動して、立ち上がった時に、パスコードを外したら
落ちなくなった(今はパスコードを設定しているが落ちない。原因はわからない)。

以下、簡単な手順。

1. iPod touch を同期(バックアップ)。この前にパスコードロックをオフにしておいた方が
いいかもしれない
2. Pwnage Tool ([The untether rolls on](http://blog.iphone-dev.org/post/4731948971/the-untether-rolls-on)
から入手)を起動。Simple Mode でデバイスを選択したら画面の指示に従う。
(注: iPhone の手順は知らない。Expert Mode だと Activate the Phone の
チェックを外さないと、電話として使えなくなるとか何とか)
3. ファームウェアがビルドされる間、しばらく待つ
4. 指示に従って iPod touch を DFU モードにする
5. iPod touch を USB 接続
6. iTunes が立ち上がったら、option キーを押しながら「復元」ボタンをクリックし、
ビルドされたファームウェアを選択
7. データの復元

その後、Cydia に moyashi リポジトリを追加した(参考: [Cydia Repo](http://hitoriblog.com/?page_id=25))。

以下、Cydia からインストールしたアプリ等。

- OpenSSH (注: mobile と root ユーザのパスワードを alpine から変更する事)
- SB Setting
    - KillBG (バックグランドで起動中のアプリをすべて終了)
- Activator
- LastApp (直前のアプリに戻る)
- Poof (ホームスクリーンから任意のアイコンを隠す)
- Springtomize Lite (アプリアイコンのラベルを消す)
- SwitcherMod (最近使ったアプリを Switcher に表示しない)
- Winter Board
    - Silent Camera (シャッター音を消す)
    - Transparent Slider Background iOS 4.3 (ロックスクリーンのスライダーの背景を消す)
    - Typophone 4
- NoSpot (Spotlight 検索を隠す)
- Lockscreen Clock Hide
- AptBackup (Cydia からインストールしたアプリのリストを作成、復元)
- iSHSHit (SHSH のバックアップ)
- Mobile Terminal (Cydia にあるやつは動かなかったので、[Download & Install Mobile Terminal 511-1 Deb iOS 4.3, 4.2.1](http://www.techpetals.com/download-install-mobile-terminal-511-1-deb-ios-4.3-4.2.1-iphone-4-ipad-ipod-touch-2953) に書かれてた 511-1 というのを入れた。起動したのは確認したが、使ってない)

Typophone 4 で時計の部分で 88:88 が点滅する問題があり、
[Typophone 4 iOS 4.2.1 FIX!](http://modmyi.com/forums/file-mods/748905-typophone-4-ios-4-2-1-fix.html)
に書かれてる方法で修正した。

`/private/var/stash/themes/typophone 4.theme` にある `Style.css` の

```
image:url(''/private/var/mobile/Library/SpringBoard/LockBackground.jpg'')
```
を
```
image:url(''LockBackground.png'')
```

に変更(実際はディレクトリ名は themes じゃなくて、themes.xxxx とかだった。xxxxは
ランダムな文字列のようだったが、人によって違うかどうかはわからない)。

Typophone 4 を入れたかったので Jailbreak してみたが、SB Setting と KillBG、
Activator と LastApp などで細かい部分の使い勝手が良くなった。
