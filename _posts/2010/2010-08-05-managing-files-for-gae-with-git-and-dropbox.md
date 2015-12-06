---
layout: post
title: Managing files for GAE with Git and Dropbox
date: '2010-08-05T22:11:17+09:00'
tags: []
---
At last, I started to use git to manage files that are hosting on GAE.
For backup and remote access, I use Dropbox for the git repository.
Here is the memo.

### Resources

- [Dropbox](http://www.dropbox.com/)
- [gitとDropboxでお手軽・無料のSource Hostingを実現する](http://naoki.sato.name/lab/archives/38)
- [Git - Fast Version Control System](http://git-scm.com/)
- [Python Application Configuration - Google App Engine - Google Code](http://code.google.com/intl/en/appengine/docs/python/config/appconfig.html)

### Steps

Actually what I did is just setting up Git in Dropbox directory.
By default, GAE ignores unix hidden files whose names begin with dot from the deployment.
I don’t need to care about files in the directory where GAE local server is referring to.

1. Download Dropbox and launch it. (Create account if you don’t have the one.)
2. Install Git. If you use Mac, you can install it with MacPorts
```sh
$ sudo port install git-core
```
3. Create a bare repository in Dropbox directory
```sh
$ cd ~/Dropbox
$ mkdir repo
$ cd repo
$ mkdir project_name.git
$ cd project_name.git
$ git --bare init
```
4. Go to the directory that contains source files, and then push the files to the git repository
```sh
$ cd ~/project_name
$ git init
$ git add .
$ git commit -m "initial commit"
$ git push ~/Dropbox/repo/project_name.git master
$ git remote add origin ~/Dropbox/repo/project_name.git
```
