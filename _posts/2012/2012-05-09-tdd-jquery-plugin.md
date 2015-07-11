---
layout: post
title: TDD で jQuery plugin を作ってみた
date: '2012-05-09T13:24:00+09:00'
tags: [jquery, plugin]
---

指定した element のサイズに合わせて 1 行ごとにフォントサイズを調整する jQuery のプラグインを書いた。結果はサンプルを見た方がわかりやすいと思う。

- [jQuery.lineJustify](https://github.com/altflow/jQuery.lineJustify) (GitHub)

指定したフォントや、使っている文字とブラウザの組み合わせによっては、上手くサイズが合わなくて折り返しちゃったりする事があるので、完璧ではないけど。。一応、Mac の Safari、Chrome、Firefox でテストした。

これを作る前に
[テスト駆動JavaScript](http://www.amazon.co.jp/gp/product/4048707868/ref=as_li_ss_tl?ie=UTF8&tag=ss0f-22&linkCode=as2&camp=247&creative=7399&creativeASIN=4048707868)
を読んでたので、JsTestDriver のテストを書きながら作ってみた。

ちょっとハマったのが、JsTestDriver の charset が iso-8859-1 で決め打ちされてるので、日本語のテストができなかった。一応 [Accepted](http://code.google.com/p/js-test-driver/issues/detail?id=85) になってる からいつかは直ると思うけど、結構放置気味…。

TDD を初めて自分でやってみたんだけど、テストを先に書く良さというのが (説明はできないまでも) 何となく体感できたような気がする。

後は、JsTestDriver のテストは、”test…” で始まらないものが無視されるようなので、考えた仕様をメモ的に “should ….” とかで空のテストとして思いついたものから書いておいて、1 つづつ順番にテスト → 実装を繰り返すようなこともした(邪道かもしれないが)。
