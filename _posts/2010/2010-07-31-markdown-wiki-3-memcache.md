---
layout: post
title: Markdown Wiki (3) Memcache
date: '2010-07-31T21:25:00+09:00'
tags: []
---
1. [ユーザーサービス](/markdown-wiki-1)
2. [データストア](/markdown-wiki-2/)
3. Memcache

### Memcache

今回勉強がてらにやってみたことの最後は Memcache。

- [Memcache Python API](http://code.google.com/intl/ja/appengine/docs/python/memcache/)

デモサイト(注: 今は削除済み)では、常に右側のメニューにページ一覧が出ているので、そのリストを取得する時に cache を利用。以下、コードの抜粋。
```python
from django.utils import simplejson
from google.appengine.ext import db
from google.appengine.api import memcache
from google.appengine.ext import webapp
from google.appengine.ext.webapp.util import run_wsgi_app

class PageList(webapp.RequestHandler):
    def get(self):
        permission = Permission()
        data       = []

        if permission.get():
            pagelist = memcache.get("pagelist")

            if pagelist is not None:
                data = pagelist
            else:
                query = WikiPage.all()
                query.order("title")

                for record in query:
                    data.append({"keyname":record.key().name(), "title": record.title })

                memcache.add("pagelist", data, 3600)

        self.response.headers[''Content-Type''] = ''text/plain''
        self.response.out.write(simplejson.dumps(data))
```

呼び出す側の JavaScript は、単に GET してるだけ。

```javascript
var Connect = YAHOO.util.Connect;

var oCallback = {
    success: _create
};
Connect.asyncRequest("GET", "/wiki/list", oCallback);
```

- [ソース](https://github.com/altflow/Markdown-Wiki)
