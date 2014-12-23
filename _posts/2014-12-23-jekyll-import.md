---
layout: post
title: "import tumblr posts with jekyll-import #1"
---

自分用のメモなども兼ねて Tumblr で blog を書いていたが、もっとシンプルにしたいとか
GitHub にアカウントがあるのにしばらく使っていないのでもったいない、などの理由で
jekyll を使って GitHub Pages に blog を移行しようと思い立った。

いろいろなアカウントを使い分けるのも面倒になってきたので、メモ的なものや
日記的なもの (これは最近 Nisshi というサービスに書いていた) を全部ここにまとめようと思う。

Jekyll への移行に伴って、[jekyll-import](http://import.jekyllrb.com/docs/tumblr/) を使って
Tumblr の post を import する試み。まずは jekyll-import のインストールから。

```sh
$ sudo gem install jekyll-import
```

その後、jekyll-import を実行 (ファイルは markdown にした)。

```sh
JekyllImport::Importers::Tumblr.run({
  "url"            => "http://stonedsoul.tumblr.com",
  "format"         => "md",
  "grab_images"    => false,
  "add_highlights" => false,
  "rewrite_urls"   => false
})'
```

エラーが出た。

```sh
/Library/Ruby/Gems/2.0.0/gems/jekyll-import-0.5.2/lib/jekyll-import/importers/tumblr.rb:135:in `post_to_hash': undefined method `<<' for false:FalseClass (NoMethodError)
from /Library/Ruby/Gems/2.0.0/gems/jekyll-import-0.5.2/lib/jekyll-import/importers/tumblr.rb:47:in `block in process'
from /Library/Ruby/Gems/2.0.0/gems/jekyll-import-0.5.2/lib/jekyll-import/importers/tumblr.rb:47:in `map'
from /Library/Ruby/Gems/2.0.0/gems/jekyll-import-0.5.2/lib/jekyll-import/importers/tumblr.rb:47:in `process'
from /Library/Ruby/Gems/2.0.0/gems/jekyll-import-0.5.2/lib/jekyll-import/importer.rb:23:in `run'
from -e:2:in `<main>'
```

調べてみたら、同じようなエラーに遭遇している人がいて、解決してたみたいだった。

- [Tumblr import fails 'post_to_hash': undefined method '<<'… #29](https://github.com/jekyll/jekyll-import/issues/29)

自分の場合は video post が引っかかってたみたいなので、該当箇所を

```ruby
when "video"
  title = post["video-title"]
  content = post["video-player"]
  unless post["video-caption"].nil?
    content << "<br/>" + post["video-caption"]
  end
```
から

```ruby
when "video"
  title = post["video-title"]
  content = post["video-player"]
  unless post["video-caption"].nil?
    unless post[:content].nil?
      content << "<br/>" + post["video-caption"]
    end
  end
```

に変えた。

とりあえず、これで全部、またはほとんどの post の .md ファイルができた。

(続く)
