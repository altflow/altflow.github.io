---
layout: post
title: jQuery mobile を使って作った、特殊な計算をするページ
date: '2012-03-12T21:53:57+09:00'
tags: []
---
[jQuery mobile](http://jquerymobile.com/) を使って何か作ろうと思い、
ちょっとした計算をするアプリを作った。[この前作ったカレンダー](/calendar)と同じようにオフラインで動く。

- [Niche Calc](https://github.com/altflow/Calc) (GitHub)

特徴:

- アスペクト比と縦か横のpxを入れるともう片方のpxを計算
- 年間コストなどを計算(金額と日次や月次などを指定すると、年間コストを計算する)
- ネットワークアドレス(xxx.xxx.xxx.xxx/xx)からIPの数などを計算
- パケットをバイトに変換
- PPI (Pixel Per Inch)を計算

後、作ってる時にいくつかハマったので、初歩的な事かもしれないが、注意点としてメモっておく。

下のような `meta` タグを入れておいて、

```html
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```

ホーム画面に webクリップとして保存したアイコンから起動すると、アプリっぽく見せる事ができるのだが、ステータスバーのスタイルを `black-translucent` にすると、コンテンツが重なっちゃうので(ヘッダーとか)、jQuery mobile を使う時は black にするか、指定しない方が良さそう。

jQuery 使って、キャッシュしたスクリプトをロードする場合、デフォルトではリクエストにタイムスタンプ付けるので、次のように cache を有効にしないとスクリプトが読めなくてエラーになる。

```javascript
$.ajaxSetup({cache: true});
$.getScript("myscript.js");
```
