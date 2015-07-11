---
layout: post
title: JavaScript の関数呼び出しと this
date: '2012-05-01T20:33:04+09:00'
tags: [javascript]
---
Yehuda Katz のブログで [2011/08/11 に post された記事](http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/) の概要。

### 大本の関数呼び出し

Function の call メソッドは次のように動作する

1. すべての引数のリストを作る (argList)
2. 最初のパラメータが thisValue
3. function が this を最初の引数、次の引数に argList を指定して呼び出される

例:
```javascript
function hello(thing) {
  console.log(this + " says hello " + thing);
}

hello.call("Yehuda", "world") //=> Yehuda says hello world
```

他の呼び出し方は、この形式に変換できる。

### 単純な関数呼び出し

例:
```javascript
function hello(thing) {
  console.log("Hello " + thing);
}

// これは:
hello("world")

// このように変換できる:
hello.call(window, "world");
```

ECMAScript 5 (ES5)の strict モードでは、 `hello.call(undefined, “world”)` になる。

### メンバ関数

例:
```javascript
var person = {
  name: "Brendan Eich",
  hello: function(thing) {
    console.log(this + " says hello " + thing);
  }
}

// this:
person.hello("world")

// desugars to this:
person.hello.call(person, "world");
```

hello メソッドがどのような方法でオブジェクトにアタッチされているかは関係ない。動的に追加された場合を見ると、次のようになる。

```javascript
function hello(thing) {
  console.log(this + " says hello " + thing);
}

person = { name: "Brendan Eich" }
person.hello = hello;

person.hello("world") // これも person.hello.call(person, "world") と同じ意味になる

hello("world") // これは "[object DOMWindow] says hello world" となる
```

### Function.prototype.bind を使った場合

常に同じ `this` を持つように、closure を使うテクニックが良く使われる。

```javascript
var person = {
  name: "Brendan Eich",
  hello: function(thing) {
    console.log(this.name + " says hello " + thing);
  }
}

var boundHello = function(thing) { return person.hello.call(person, thing); }

boundHello("world"); // "Brendan Eich says hello world"
```

このテクニックをもう少し汎用的にして、次のように書くことができる。

```javascript
var bind = function(func, thisValue) {
  return function() {
    return func.apply(thisValue, arguments);
  }
}

var boundHello = bind(person.hello, person);
boundHello("world") // "Brendan Eich says hello world"
```

これを理解するためのポイントは、`arguments` は配列に似たオブジェクトだということと、`apply`
メソッドは引数を配列に似たオブジェクトとして扱う以外は call メソッドと同じだということ。

尚、ES5 には `Function` に `bind` メソッドが追加されている。

```javascript
var boundHello = person.hello.bind(person);
boundHello("world") // "Brendan Eich says hello world"
```

これはもとの関数をコールバックとして渡す時に便利である。

```javascript
var person = {
  name: "Alex Russell",
  hello: function() { console.log(this.name + " says hello world"); }
}

$("#some-div").click(person.hello.bind(person));

// div がクリックされたら、 "Alex Russell says hello world" が表示される
```
