---
layout: post
title: location object と anchor element
date: '2013-09-21T21:19:14+09:00'
tags: [javascript]
---
JavaScript で Unit Test を書こうとした時、その関数が location object を
処理したり、URL の文字列からホスト名などを取得したい場合、anchor element を生成して location object の代わりに使うという方法が stack overflow にあった。

testem とか使ってテストをする場合、location.href に値を入れると画面遷移してしまうので、別のオブジェクトを用意して使えばよい。

例: URL 文字列からホスト名を取得

```javascript
var mylocation = document.createElement(''a'');
mylocation.href = ''http://example.com/path/to/file'';

var hostname = mylocation.hostname;
```

例: アクセスしているURLのパラメータを削除した文字列を返す関数とそのテスト

```javascript
// Test
describe(''callMyFuncTest'', function(){
    beforeEach(function(){
        myNS.location = document.createElement(''a'');
    });

    afterEach(function(){
        myNS.location = window.location;
    });

    it(''should return url without query string'', function(){
        myNS.location.href = ''http://example.com/path?query=string'';

        expect(myNS.removeQueryStr()).toBe(''http://example.com/path'');
    });
});

// Function
var myNS = {
    location: window.location
};

myNS.removeQueryStr = function(){
    var l = myNS.location;

    return l.protocol+''://''+l.hostname+l.pathname;
}
```

コードの方で少し工夫する必要はあるけど、一応これでテストできる。

参照: MDN

- [Web API interfaces > Location](https://developer.mozilla.org/en-US/docs/Web/API/Location)
- [Web API interfaces > HTMLAnchorElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLAnchorElement)
