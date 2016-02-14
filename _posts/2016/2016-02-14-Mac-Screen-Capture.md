---
layout: post
title: Mac で画面動画キャプチャ
date: '2016-02-14T14:37:00+09:00'
tags: []
---

そんなに頻繁にやらないので、いつも手順を忘れてしまうため、ここにメモ。

参考: [Soundflowerを用いて音声入りの画面キャプチャを撮る方法](http://qiita.com/Sasakky/items/09e3bc1536f0569fc893)

手順:

1. [Soundflower](https://github.com/mattingalls/Soundflower/releases/tag/2.0b2)をインストール
2. Soundflowerを起動
3. Soundflower 2ch の Build-in Output を選択する(Noneだと録画中にスピーカーから音が出ないが、録音はされる)
4. メニューバーの音量を Option キーを押しながらクリックし、入力、出力を Soundflower 2ch へ変更
5. Quick Time Playerを起動
6. File > 新規画面収録
7. マイクを Soundflower (2ch) へ変更し、Macの音量を最大に変更(録画ボタンの下の音量は最小のままで良い)
8. 録画開始
