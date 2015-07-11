---
layout: post
title: Foundation フレームワーク
date: '2012-05-06T19:47:31+09:00'
tags: []
---
Twitter の [Bootstrap](http://twitter.github.com/bootstrap/) が人気みたいだが、[Wireframes Magazine](http://wireframes.linowski.ca/2012/04/foundation/) で同じようなフレームワークの Foundationというのが紹介されていた (MIT License)。

- [Foundation](http://foundation.zurb.com/)
- [GitHub](https://github.com/zurb/foundation)

UIのコンポーネントとしては一通りのものがそろっている。特徴は responsive design に対応しているところ。以下のサイトにレビューやコメントがある。

- [Smashing Magazine](http://coding.smashingmagazine.com/2011/10/25/rapid-prototyping-for-any-device-with-foundation/)
- [CSS-Tricks Forums](http://css-tricks.com/forums/discussion/14293/anyone-using-foundation-from-zurb-yets/p1)

好みにもよるが、ネガティブなコメントもあった。例えば、

- Form のスタイルはクラスで指定するのではなく、全体に適用される
- normalize じゃなくて reset している
- モバイル向けにコンテンツの一部を隠したりするのを CSS ベースで行っているので、読み込み自体はされてしまっている
- イメージのリサイズについても同様

まあ Rapid Prototyping and Building Framework とあるように、細かい部分まで考慮するなら、いろいろカスタマイズしてサーバ側のスクリプトも使ったり、複数の画像を用意したりする必要があるのかも知れないが、デモページをいくつかブラウザのウィンドウサイズを変えながら見たりした限りでは、なかなか便利そうだった。例えば[これ](http://foundation.zurb.com/mobile-example3.php)。
