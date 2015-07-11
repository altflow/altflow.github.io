---
layout: post
title: 'iPhone でウェブページをアプリっぽく表示する '
date: '2012-01-11T23:01:38+09:00'
tags: []
---
何か作ろうと思って、ちょっと調べたのでメモ。

参考:

- [iphone対応サイト制作まとめ](http://www.ame-nochi-hare.com/2010/03/08/iphone-site-build/)
- [Viewport (iPhone生活)](http://ipn3g.com/web/study3.html)
- [iPhone / iPad サイズ別 apple-touch-icon.png 作成](http://blog.fonland.net/2011/02/iphone-ipad-apple-touch-iconpng.html)

### meta タグ
```html
<meta name="viewport" content="width=device-width , user-scalable=no , inicial-scale=1 , maximum-scale=1" />
```

ページのサイズや拡大縮小できるかを指定。
```html
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```

フルスクリーンモードにするかどうかと、ステータスバーのスタイル。

### アイコン
```html
<link rel="apple-touch-icon" href="img/apple-touch-icon.png" />
<link rel="apple-touch-icon" sizes="72x72" href="img/apple-touch-icon-72.png" />
<link rel="apple-touch-icon" sizes="114x114" href="img/apple-touch-icon-114.png" />
```

3G、iPad、4 (retina)それぞれ別に指定できる。ファイル名に “precomposed” と付けると、光沢処理がされない (例: apple-touch-icon-precomposed.png)

### アドレスバーを隠す
```javascript
function hideAddressBar(){
    setTimeout(scrollTo(0,1),500);
};

$(document).ready(function(){ hideAddressBar(); });
$("body").bind("orientationchange", hideAddressBar);
```

JavaScriptでスクロールさせる。

### height 100% の box を作る

HTML:
```html
<!DOCTYPE HTML>
<html lang="ja-JP">
<head>
    <meta charset="UTF-8">
    <title>test</title>
</head>
<body>
<div id="container">
</div>
</body>
</html>
```

CSS:
```css
* {
    margin: 0;
    padding: 0;
}

html {
    height: 100%;
}

body {
    height: 100%;
}

#container {
    height: 100%;
    min-height: 100%;
}
```
