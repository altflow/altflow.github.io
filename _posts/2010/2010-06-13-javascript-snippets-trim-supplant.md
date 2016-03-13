---
layout: post
title: JavaScript snippets (trim, supplant)
date: '2010-06-13T22:32:07+09:00'
tags: []
---
[17 Hours of JavaScript from the Masters](http://code.tutsplus.com/articles/17-hours-of-javascript-from-the-masters--net-7615) で紹介されてるビデオの The JavaScript Programming Language の Part 3 にでてきたコードをメモ。

12:40 あたりに出てくる trim

```javascript
String.prototype.trim = function() {
    return this.replace(
        /^\s*(\S*(\s+\S+)*)\s*$/, "$1");
};
```

ちなみに [Snipplr](http://snipplr.com) で [JavaScript trim](http://snipplr.com/search.php?q=javascript%20trim) で検索するといくつかヒットするけど、それぞれ微妙にやり方が違ってて面白い。

その後、13:00 あたりから出てくる supplant

```javascript
String.prototype.supplant = function(o) {
    return this.replace(/{([^{}]*)}/g,
        function (a, b) {
            var r = o[b];
            return typeof r === 'string' ? r : a;
        }
    );
};
```

それの使い方(ビデオでは使い方が先に出てくる)。

```javascript
var template = '<table border="{border}">' +
    '<tr><th>Last</th><td>{last}</td></tr>' +
    '<tr><th>First</th><td>{first}</td></tr>' +
    '</table>';

var data = {
    first: "Carl",
    last: "Hollywood",
    border: 2
};

mydiv.innerHTML = template.supplant(data);
```

同じようなものに RND template があって、これを参考に(ほとんどコピペして)JavaScriptで最低限のテンプレートシステムを書いたりしたけど、やっぱり書き方が微妙に違う。
