---
layout: post
title: Markdown Wiki (1) ユーザー認証
date: '2010-07-31T21:22:00+09:00'
tags: []
tumblr_url: http://stonedsoul.tumblr.com/post/884123105/markdown-wiki-1
---
Google App Engine の勉強がてら、シンプルな wiki を作ってみた。といっても、主に GAE の

1. ユーザーサービス
2. [データストア](/markdown-wiki-2/)
3. [Memcache](/markdown-wiki-3/)

の使い方を知るためのもので、ページのレンダリングは [Showdown](http://attacklab.net/showdown/) を使った。サーバ側のスクリプトは Python で、リクエストを受けた結果を JSON で返している。

GAE だけじゃなくて、Python の勉強もかねているので、変なところがあるかもしれない。。

### ユーザーサービス

基本的な使い方は、GAE のドキュメント [ユーザー サービスの使用](http://code.google.com/intl/ja/appengine/docs/python/gettingstarted/usingusers.html)にあるように、簡単だった。これだと Gmail のアカウントを持ってる人は誰でもアクセスできる事になるので、メールアドレスのリストを用意して、そこに登録されている人のアクセスを許可するようにした。該当部分を一部抜粋。

```python
from google.appengine.api import users
from google.appengine.ext import webapp
from google.appengine.ext.webapp.util import run_wsgi_app

class Permission:
    def __init__(self):
        self.accessible = ["example@gmail.com"]

    def get(self):
        user = users.get_current_user()

        if user:
            if len(self.accessible) < 1:
                return True

            for email in self.accessible:
                if user.email() == email:
                    return True

        return False

class Auth(webapp.RequestHandler):
    def get(self):
        url        = ''/wiki.html''
        permission = Permission()

        if permission.get():
            user = users.get_current_user()
            data = {"nickname": user.nickname(), "logoutUrl": users.create_logout_url(url)}

        else:
            data = {"loginUrl": users.create_login_url(url)}

        self.response.headers[''Content-Type''] = ''text/plain''
        self.response.out.write(simplejson.dumps(data))
```

JavaScript 側から以下のようにして呼び出している(JavaScript ライブラリは YUI を使用)。

```javascript
YAHOO.namespace("mdwiki");

YAHOO.mdwiki.Auth = function(){
    var Connect = YAHOO.util.Connect,
        Dom     = YAHOO.util.Dom,
        Event   = YAHOO.util.Event,
        JSON    = YAHOO.lang.JSON;

    var _handleAuth = function(oResponse){
        var oAuthInfo = JSON.parse(oResponse.responseText);

        if (typeof oAuthInfo === "object") {
            var oProfile   = Dom.get("profile");

            if (oAuthInfo.nickname) {
                // the user has been authenticated
                oProfile.innerHTML = "<a href=''" + oAuthInfo.logoutUrl + "''>Logout</a>";
                YAHOO.mdwiki.Page.show();
                YAHOO.mdwiki.PageList.init();
            } else {
                // the user has not been authenticated
                oProfile.innerHTML = "<a href=''" + oAuthInfo.loginUrl + "''>Login</a>";
            }
        }
    };

    Event.onDOMReady(function(){
        var oCallback = {
            success: _handleAuth
        };
        Connect.asyncRequest("GET", "/wiki/auth", oCallback);
    });
}();
```

長くなりそうなので、[その 2](/markdown-wiki-2/) へつづく。。

- [ソース](https://github.com/altflow/Markdown-Wiki)
