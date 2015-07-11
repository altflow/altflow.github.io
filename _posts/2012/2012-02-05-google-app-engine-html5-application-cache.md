---
layout: post
title: Google App Engine で HTML5 の Application Cache
date: '2012-02-05T14:19:59+09:00'
tags: []
---
参考:

- [iPhoneでHTML5のアプリケーションキャッシュを利用する](http://blog.monoweb.info/article/2010091522.html)
- [アプリケーションキャッシュの使い方](http://tenderfeel.xsrv.jp/html-xhtml/html5-html-xhtml/1172/)
- [GAE (Google App Engine) で Cache Manifest を使うには](http://d.hatena.ne.jp/ambasa/20110304/p1)
- [HTML5的にmanifestファイルをゴニョった](http://havelog.ayumusato.com/develop/html/entry-69.html)

### マニフェストファイルを用意する

拡張子は .appcache (.manifestから変更された)。以下サンプル。

```
CACHE MANIFEST
# version 1.0

CACHE:
path/to/the/file.html
/could/be/absolute/path.js

NETWORK:
always/refer/to/serverside/content.html

# external source
http://example.com/
```

サーバを参照するものがある場合は、必ず NETWORK に指定(CACHEで指定したもの以外はサーバを参照する、という事ではない)。

マニフェストファイルが更新されないとブラウザはキャッシュし続ける。ファイルの構成が変わらなくても更新させたい場合を考えて、コメントでバージョンとか日付とか入れておき、キャッシュを更新させる場合はここを変更する。

### HTMLとJavaScript

HTMLタグにmafifest属性を追加。

```html
<html manifest="example.appcache">
```

JavaScriptでキャッシュ対象のファイルがアップデートされた時に更新する。

```javascript
applicationCache.addEventListener("updateready", function(){
    applicationCache.swapCache();
}, false);
```

### GAEの設定 (app.yaml)

mime_typeを text/cache-manifestにする。以下、例。

```
- url: /(.*)\.appcache
  static_files: static/\1.appcache
  upload: static/(.*)\.appcache
  mime_type: text/cache-manifest
```
