---
layout: post
title: Team development of JavaScript
date: '2010-09-27T12:08:25+09:00'
tags: []
---
I saw an entry about some practices of team development of JavaScript by amachang.
While several years ago, I saw a similar blog post and I remember that’s very
useful for me and I thought that I would post the summary in English, but I neglected
them so far. Now I summarize them here for my personal study.

- [サイボウズで学んだこと - IT戦記](http://d.hatena.ne.jp/amachang/20100917/1284700700)

There are three points in the blog post.

- How to share the code in a team though JavaScript is flexibile in the coding style
- How to debug the code
- Performance tuning

### How to share the code in a team

- Standardize basic library. (e.g., jQuery, Prototype.js, etc.) That means use one
basic library in a page.
- Have naming convention (for example, class name must be written in camel case, use
prefix ‘opt_’ for arguments which is omissible.)
- Delegate resolving dependency to library
- Emphasize to illustrate concepts to understand it in the team (class oriented style
should be good to illustrate the concept in UML)
- Use same form, pattern of coding for complicated process (e.g., deferred)

In another blog [ハタさんのブログ(復刻版) : Javascriptによる大規模開発の覚え書き](http://blog.xole.net/article.php?id=612),
there are some other good practices in terms of team development.

- Use JSON (hash) for arguments, so that you don’t have to care its order
when you call the function
- Write document with JsDoc (or other documentation tool like [jsdoc-toolkit](http://code.google.com/p/jsdoc-toolkit/))

### How to debug

- Use assertion. There is an example of function assert which call debugger when
the precondition is not filled and its usage (sample) in the page.
- Use conditional breakpoint instead of other debug (e.g., alert, console.log.)

### Performance tuning

- You don’t need to care about performance from the beginning
- But don’t do make it much slower (e.g., scanning all DOM elements)
- Focus sensible speed first  (e.g., give the user enough feedback from the UI. Avoid too much animation.)
- Use profiler (firebug, webkit) to figure out the point of performance problem

The author recommend to log the processing time at the first item of the post.
