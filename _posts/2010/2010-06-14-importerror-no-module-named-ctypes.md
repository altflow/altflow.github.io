---
layout: post
title: 'ImportError: No module named _ctypes'
date: '2010-06-14T14:53:10+09:00'
tags: []
---
I got the error when I ran my application on localhost which was started via Google App Engine Launcher. It seems to occur on Max OSX and Python 2.6. To fix that, set python path in preference of GAE Launcher as below instruction.

[Issue 985 - googleappengine - Import Error: Failed to import ctypes to load dll on windows - Project Hosting on Google Code](https://code.google.com/p/googleappengine/issues/detail?id=985)


> Comment 10 by mike.j.dobbs, Feb 11, 2010
>
> Changing preferences—>”my python path” to “/usr/bin/python2.5” and restarting
> GoogleAppEngineLauncher fixed this on my Snow Leopard.
>
> Comment 11 by kmallea, Mar 22, 2010
>
> Make sure you hit {enter} in the input field, or the Python Path you type in will not
> save. I believe this is a bug in the GAE GUI; I read this somewhere else, I forgot
> where exactly.


(Japanese)

OSX で GoogleAppEngineLauncher を使ってて動作確認する時に urllib を使ってる部分でエラーがでた。回避策は上のページにあるように Python 2.5 を使う事だけど、GAE Launcher の設定画面で Python の Path を設定する時は、入力したら Enter キーを押さないと設定が反映されない、というところではまった。。
