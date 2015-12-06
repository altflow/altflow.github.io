---
layout: post
title: Markdown Wiki (2) データストア
date: '2010-07-31T21:23:00+09:00'
tags: []
---
1. [ユーザーサービス](/markdown-wiki-1/)
2. データストア
3. [Memcache](/markdown-wiki-3/)

### データストア

Google App Engine の勉強のつづきで、データストアの使い方。GAE のドキュメントは、

- [データストアの使用](http://code.google.com/intl/ja/appengine/docs/python/gettingstarted/usingdatastore.html)
- [Python Datastore API](http://code.google.com/intl/ja/appengine/docs/python/datastore/)

などを参照した。

GQL という SQL のようなクエリ言語を使ってデータを扱えるけど、それとは別の Query インターフェースを使った。以下はデモサイトでページを表示、編集する部分のコード。

```python
from django.utils import simplejson
from google.appengine.ext import db
from google.appengine.ext import webapp
from google.appengine.ext.webapp.util import run_wsgi_app

class Page(webapp.RequestHandler):
    def __init__(self):
        self.permission = Permission()
        self.data       = {}

    def get(self, keyname):
        if self.permission.get():
            page = WikiPage.get_by_key_name(keyname)
            self.data = {"keyname":keyname, "title": page.title, "content": page.content}

        self.response.headers[''Content-Type''] = ''text/plain''
        self.response.out.write(simplejson.dumps(self.data))

    def post(self, keyname):
        if self.permission.get():
            page         = WikiPage(key_name=keyname)
            page.title   = self.request.get("title")
            page.content = self.request.get("content")
            page.put()

            self.data = {"keyname":keyname, "title": page.title, "content": page.content}

        self.response.headers[''Content-Type''] = ''text/plain''
        self.response.out.write(simplejson.dumps(self.data))
```

以下のような JavaScript から呼び出される (ページ更新時のリクエスト部分のみ。JavaScript ライブラリは YUI)。

```javascript
var Connect = YAHOO.util.Connect,
    Dom     = YAHOO.util.Dom;

var _onClickUpdate = function(oEvent){
    var sKeyName  = Dom.get("keyname").value,
        sTitle    = Dom.get("title").value,
        sPostData = "title=" + encodeURIComponent(sTitle)
                  + "&content=" + encodeURIComponent( Dom.get("content").value ),
        oCallback = {
            success: _renderPage
        };
    Connect.asyncRequest("POST", _sRequestUrl + sKeyName, oCallback, sPostData);
    Dom.get(sKeyName).innerHTML = sTitle;
};
```

[その 3](/markdown-wiki-3/) へつづく。。

- [ソース](https://github.com/altflow/Markdown-Wiki)
