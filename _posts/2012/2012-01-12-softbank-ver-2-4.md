---
layout: post
title: SoftBankメール ver.2.4 の不具合
date: '2012-01-12T22:12:49+09:00'
tags: []
---
普段携帯でメールしないから全然気づかなかったが、2011/12/21頃にアップデートした Android の [SoftBank メール](https://market.android.com/details?id=jp.softbank.mb.mail) でMMSの送受信ができなくなってた。SMSは問題なかった。参ったと思って調べたら、価格.com の掲示板に対処法が書いてあって、それを試したら直った。

[Softbankメールアプリで不具合！](http://bbs.kakaku.com/bbs/K0000157145/SortID=13938565/)

> 以下の手順でアクセスポイントを初期化してみてください。
>
>  [menu]ボタン →設定 →無線とネットワーク →モバイルネットワーク設定 →アクセスポイント名 →[menu]ボタン →初期設定にリセット

SIMロックフリーの端末使ってる人は、アクセスポイントを設定し直す必要があるから、ちょっと手間がかかる。

参考:

- [Ｂ級くおりてぃ](http://bqolife.blog53.fc2.com/blog-entry-204.html)
- [Nexus Oneレビュー(ドコモ、ソフトバンク)](http://www.yasukawa.com/blog/archives/000398.html)
- [APN for Internet & MMS .apk for Softbank users in JAPAN](http://android.modaco.com/topic/301380-apn-for-internet-mms-apk-for-softbank-users-in-japan/)

などなど。

(銀SIMの場合)

- Name: (任意)
- APN:open.softbank.ne.jp
- Proxy: (未設定)
- Port: (未設定)
- Username: opensoftbank
- Password: (自粛)
- Server: (未設定)
- MMSC:http://mms/
- MMS: mmsopen.softbank.ne.jp
- MMS Port: 8080
- MMS protocol: WAP2.0
- MCC: 440
- MNC: 20
- Authentication type: (未設定)
- APN type: (未設定)

(水色SIMだと APN とかが違うらしい。)
