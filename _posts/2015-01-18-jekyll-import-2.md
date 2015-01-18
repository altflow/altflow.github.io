---
layout: post
title: "import tumblr posts with jekyll-import #2"
---

[前回](http://altflow.github.io/jekyll-import/) の続き。

前回の方法で [jekyll-import](http://import.jekyllrb.com/docs/tumblr/) を使って
export したファイルを見ると、

- 見出し
- 箇条書き
- リンク
- コードブロック

が無くなって、段落以外はフラットなテキストになってた。
元々、移行する post を選択するつもりでいたので、１つ１つ見ていくことにした。

移行自体は、単純に .md ファイルをフォルダに移して commit するだけ。
