---
layout: post
title: はじめての jQuery Plugin
date: '2012-02-05T13:20:00+09:00'
tags: [jquery, plugin]
---
[前回の post](/javascript-flick-event) にフリックのイベントについて書いたが、イベントを検知する jQuery Plugin にしてみた。[jQuery Touchwipe](http://www.netcu.de/jquery-touchwipe-iphone-ipad-library) とほとんど一緒なんだけど、[jQuery Mobile](http://jquerymobile.com/) も参考に、touchだけじゃなくてmouseでも使えるようにした。

### Pluginのテンプレート

```javascript
(function($){
    $.fn.myplugin = function(option){
        // default values
        var config = {
            config1: "value"
        };

        // override config with the give option(s)
        if (option) {
            $.extend(config, option);
        }

        // "this" is a jQuery object
        this.each(function(){
            // in the function, "this" is a DOM element
            // set shorthand of jQuery object if necessary
            var $this = $(this);

            // do something
        });

        // return "this" not to break method chain
        return this;
    };
})(jQuery);
```

シンプルな Plugin なら、これの必要な箇所に必要な処理を入れてけばいい。
作り方を解説するみたいなタイトル付けたけど、あんまり解説する事なくて、テンプレメモだ。。

参考:

- [jQueryプラグインの作り方](http://phpjavascriptroom.com/?t=ajax&p=jquery_plugin)
- [Plugins/Authoring (jquery.com)](http://docs.jquery.com/Plugins/Authoring)

### 今回作ったやつ (jQuery.swipeListener)

- [jQuery.swipeListener](https://github.com/altflow/jQuery.swipeListener) (GitHub)

```javascript
(function($){

    $.fn.swipeListener = function(option){
        var config = {
            duration: 1000, // within this period
            minX: 20, // swipe L/R if touch position moved more than this horizontally
            minY: 20, // swipe U/D if touch position moved more than this vertically
            swipeUp: undefined,   // callback function invoked when swipe up
            swipeDown: undefined, // or swipe down,
            swipeLeft: undefined, // or swipe left,
            swipeRight: undefined // or swipe right
        };

        if (option) {
            $.extend(config, option);
        }

        this.each(function(){
            var start = undefined;
            var stop  = undefined;
            var $this = $(this);
            var isTouchDevice   = typeof this.ontouchstart !== "undefined";
            var touchStartEvent = isTouchDevice ? "touchstart" : "mousedown";
            var touchMoveEvent  = isTouchDevice ? "touchmove" : "mousemove";
            var touchEndEvent   = isTouchDevice ? "touchend" : "mouseup";

            $this.bind(touchStartEvent, touchStart);

            function touchStart(event){
                var e = isTouchDevice ? event.originalEvent.touches[0] : event;
                start = {
                    x: e.pageX,
                    y: e.pageY,
                    time: (new Date()).getTime()
                };
                event.stopPropagation();

                $this.bind(touchMoveEvent, touchMove).one(touchEndEvent, touchEnd);
            };

            function touchMove(event){
                if (!start) {
                    return;
                }

                event.preventDefault();

                var e = isTouchDevice ? event.originalEvent.touches[0] : event;
                stop = {
                    x: e.pageX,
                    y: e.pageY,
                    time: (new Date()).getTime()
                };
            };

            function touchEnd(event){
                $this.unbind("touchmove mousemove", touchMove);

                if (start && stop && stop.time-start.time < config.duration) {
                    diffX = start.x - stop.x;
                    diffY = start.y - stop.y;

                    if (Math.abs(diffX) > config.minX) {

                        if (diffX > 0 && config.swipeLeft) {
                            config.swipeLeft();
                        } else if (config.swipeRight) {
                            config.swipeRight();
                        }

                    } else if (Math.abs(diffY) > config.minY) {

                        if (diffY > 0 && config.swipeUp) {
                            config.swipeUp();
                        } else if (config.swipeDown) {
                            config.swipeDown();
                        }
                    }
                }

                start = stop = undefined;
            };
        });

        return this;
    };

})(jQuery);
```
