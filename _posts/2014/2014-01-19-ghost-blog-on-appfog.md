---
layout: post
title: Ghost blog on appfog
date: '2014-01-19T20:20:50+09:00'
tags: []
tumblr_url: http://stonedsoul.tumblr.com/post/73819796960/ghost-blog-on-appfog
---
Node.js で動く[Ghost](https://ghost.org/) を [appfog](https://www.appfog.com/) で動かすことができたので、メモ。

基本的には以下のページにある手順で行う。

- [It’s here, my Ghost blog](http://joakimbeng.eu01.aws.af.cm/its-here-my-ghost-blog/)

このページに書かれていることで、それ以外に設定すべきところは次の通り

### Env Variables

appfog のコンソールで Env Variables に Name “NODE_ENV”, Value “production” を設定

#### config.js

server 部分を次のように修正。

```javascript
server: {
  // Host to be passed to node''s `net.Server#listen()`
  host: ''0.0.0.0'',
  // Port to be passed to node''s `net.Server#listen()`, for iisnode set this to `process.env.PORT`
  port: process.env.VCAP_APP_PORT
}
```

#### .afignore

Ghost に限ったことではないが、appfog で node.js を使う時は、node_modules/.bin を指定した方が良いらしいので、一応しておく。

```
# ignore mode_modules/.bin
node_modules/.bin/
```

これで動いた。

### その他

メール送信は Ghost のサイトにあるドキュメントを見ればできる(Gmail を使う方法もある)。

- [Setting up Email](http://docs.ghost.org/mail/)

間抜けなことに、Ghost をセットアップした時に設定したメールアドレスがわからなくなってしまったので、appfog 以下のページにある方法で mysql の users テーブルを確認したりした。。

- [MySQL](https://docs.appfog.com/services/mysql) (tunneling して Execute Query を使ってアクセスした)
