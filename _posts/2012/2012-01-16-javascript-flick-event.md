---
layout: post
title: JavaScript のフリックイベントでカードをめくる
date: '2012-01-16T22:30:02+09:00'
tags: []
---
参考:

- [jQuery.flickSimple 縦フリックに対応しました。](http://d.hatena.ne.jp/makog/20111009/1318175244)
- [jQuery Touchwipe Plugin](http://www.netcu.de/jquery-touchwipe-iphone-ipad-library)
- [JavascriptとCSSアニメーションでフリック操作を実装してみる 続きの続き](http://dev.worksap.co.jp/Members/nogunogu/2010/07/14/iphonesafari-javascriptとcssアニメーションでフリック操作を実装し-3/)
- [jQuery Mobile](http://jquerymobile.com/)
- [Super Cool CSS Flip Effect with Webkit Animation](http://line25.com/articles/super-cool-css-flip-effect-with-webkit-animation) (via [カードをぺろっと裏返すエフェクトを実装するスタイルシート](http://coliss.com/articles/build-websites/operation/css/css-tutorial-flip-effect-with-webkit-animation.html))

### フリック(またはスワイプ)イベント

ちょっと検索してみると、onFlickとかonSwipeとかいう都合のいいイベントハンドラは無いので、touch関係のイベント

- touchstart
- touchmove
- touchend
- touchcancel
- gesturestart
- gesturechange
- gestureend

を使って、タッチしてから放すまでの間に、どのくらい指が動いたかをチェックして、指定した距離以上移動したら(場合によっては何秒以内に、という条件をつけて)、フリック(スワイプ)したと判断する。

シンプルにイベントハンドラだけ実装してるjQuery Touchwipe Pluginは、`touchstart` と `touchmove` だけチェックしてる。フリックしてエレメントを動かす(切り替える)のが目的の jQuery.flickSimple は `touchend` もチェックしている。「実装してみる」の記事や jQuery Mobile では、それに加えて `touchstart` から `touchend` までの時間もチェック(素早く動かした時をフリックと判断)。

### カードを捲る

表用と裏用のコンテンツを用意して重ねて表示しておき、回転させた時にそれぞれの裏面が見えないように `backface-visiblity` を `hidden` にする。裏面用のコンテンツはあらかじめ `rotateY(180deg)` として裏にひっくり返しておく。

横向きのフリックイベントが発生した時に、カードに対して `class` を適用してアニメーションさせる。

### サンプル

HTML:
```html
<!DOCTYPE HTML>
<html lang="ja-JP">
<head>
    <meta charset="UTF-8">
    <title>flick test</title>
    <link rel="stylesheet" type="text/css" href="flicktest.css" media="all" />

</head>
<body>

<div id="card">
    <div id="face">
        <div id="front">F</div>
        <div id="back" class="reversed">B</div>
    </div>
</div>

<script type="text/javascript" src="http://code.jquery.com/jquery-1.7.1.min.js"></script>
<script type="text/javascript" src="flicktest.js"></script>
</body>
</html>
```

CSS:
```css
#card {
    border: 1px solid #999;
    width: 240px;
    height: 320px;
    -webkit-perspective: 1000;
    -webkit-transform-style: preserve-3d;
}

#face {
    padding: 5px;
    width: 240px;
    height: 320px;
}

#front {
    background: #ccd;
    position: absolute;
    width: 230px;
    height: 310px;
    -webkit-backface-visibility: hidden;
}

#back {
    background: #dcc;
    width: 230px;
    height: 310px;
    position: absolute;
    -webkit-backface-visibility: hidden;
}

.reversed {
    -webkit-transform: rotateY(180deg);
}


/** Animation: Turn Left **/
.turnleft {
    -webkit-animation-duration: 300ms;
    -webkit-animation-name: turnleft;
}

@-webkit-keyframes turnleft {
    0% { -webkit-transform: rotateY(0deg); }
    100% { -webkit-transform: rotateY(180deg); }
}

/** Animation: Turn Right **/
.turnright {
    -webkit-animation-duration: 300ms;
    -webkit-animation-name: turnright;
}

@-webkit-keyframes turnright {
    0% { -webkit-transform: rotateY(0deg); }
    100% { -webkit-transform: rotateY(-180deg); }
}
```

JavaScript:

```javascript
$(document).ready(function(){
    var cardEl  = $("#card");
    var frontEl = $("#front");
    var backEl  = $("#back");
    var touchDevice = typeof cardEl[0].ontouchstart !== "undefined";

    var startX = null;
    var minX   = 40;

    cardEl.bind("touchstart mousedown", touchStart);

    function touchStart(e) {
        e.stopPropagation();

        var ev = touchDevice ? e.originalEvent.touches[0] : e;
        startX = ev.clientX;

        cardEl.bind("touchmove mousemove", touchMove);
    }

    function touchMove(e) {
        e.preventDefault();
        var ev = touchDevice ? e.originalEvent.touches[0] : e;
        var diffX = ev.clientX - startX;

        if (Math.abs(diffX) > minX) {
            startX = null;
            cardEl.unbind("touchmove mousemove");
            turnCard(diffX);
        }
    }

    function animationEnd(e) {
        cardEl.removeClass();
        frontEl.toggleClass("reversed");
        backEl.toggleClass("reversed");
        cardEl.unbind("webkitAnimationEnd");
    }

    function turnCard(diffX) {
        cardEl.bind("webkitAnimationEnd", animationEnd);

        if (diffX < 0) {
            cardEl.addClass("turnleft");
        } else {
            cardEl.addClass("turnright");
        }
    }
});
```
