---
layout: post
title: JavaScript で CSV をテンプレートにあわせて 1 行 1 ファイルに変換
date: '2013-04-14T00:06:08+09:00'
tags: []
---

例えば以下のような CSV から

```csv
title,date,text
Convert CSV into some files,2013-04-12,How to convert each record in CSV to...
Sample script,2013-04-11,Here is the sample script that you want...
```

以下の様なテンプレートを使って

```html
<html>
<head>
    <title>{{title}}</title>
</head>
<body>
    <h1>{{date}} {{title}}</h1>
    <p>{{text}}</p>
</body>
</html>
```

以下のようなファイルを生成するスクリプト(各行につき 1 ファイル)を書いた。自分以外に必要とする人がいるか分からないが。。。

```html
<html>
<head>
    <title>Convert CSV into some files</title>
</head>
<body>
    <h1>2013-04-12 Convert CSV into some files</h1>
    <p>How to convert each record in CSV to...</p>
</body>
</html>
```

Node.js と mu、fast-csv モジュールを使う。

```sh
$ npm install mu2
$ npm install fast-csv
```

convert.js

```javascript
var mu  = require(''mu2'');
var csv = require(''fast-csv'');
var fs  = require(''fs'');

var jsondata = [];
var contents = [];
var datafile = ''data.csv'';
var template = ''template.html'';
var outfile  = ''page'';

csv(datafile, {headers: true})
.on(''data'', function(data){
    jsondata.push(data);
})
.on(''end'', function(){
    for (var i=0, l=jsondata.length; i<l; i++) {
        render(template, jsondata[i], i);
    }
})
.parse();

function render (template, data, contentIndex) {
    contents[contentIndex] = "";

    mu.compileAndRender(template, data)
    .on(''data'', function(output){
        contents[contentIndex] += output.toString();
    })
    .on(''end'', function(){
        fs.writeFileSync(outfile+''_''+contentIndex+''.html'', contents[contentIndex]);
    });
}
```

ちょっと必要になったので作ってみた。そして後で少しいじって使うかもしれないので、メモとして残してみた。
