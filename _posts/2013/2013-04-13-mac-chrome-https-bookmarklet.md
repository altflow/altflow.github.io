---
layout: post
title: Mac の Chrome で https のページ上で bookmarklet を使う
date: '2013-04-13T23:44:47+09:00'
tags: [mac]
---

Chrome ではセキュリティの関係で https のページでは bookmarklet が動かない。
Windows だとショートカットにオプションをつけてあげれば良いが、Mac だと同じようにはできない。

そこで以下のようにファイルを作りアプリっぽい形にしてみた。

```
chrome(insecure).app/
 +- Contents/
     +- MacOS/
     |   +- OpenChrome.sh
     +- Resources/
     |   +- app.icns
     +- Info.plist
```

OpenChrome.sh

```sh
#!/bin/sh
''/Applications/Google Chrome.app/Contents/MacOS/Google Chrome'' --allow-running-insecure-content
```

Info.plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>CFBundleExecutable</key>
    <string>OpenChrome.sh</string>
    <key>CFBundleIconFile</key>
    <string>app.icns</string>
    <key>CFBundlePackageType</key>
    <string>APPL</string>
  </dict>
</plist>
```

app.icns は、適当に検索して [IconArchive](http://www.iconarchive.com/tag/google-chrome) から Chromium Icon を拾ってきて使っている。
