---
layout: post
title: "Liquid Exception in octopress"
date: 2014-02-18 19:04:11 +0900
comments: true
categories: octopress
---

octopressを使ったブログを更新してる時に下記のようなエラーに出くわした。


```
{% raw %} Liquid Exception: Variable '{{status: result, error: []}' was not properly {% endraw %}
terminated with regexp: /\}\}/  in
2014-02-04-get-and-post-method-for-sinatra.markdown
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/liquid-2.3.0/lib/liquid/block.rb:78:in
`create_variable'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/liquid-2.3.0/lib/liquid/block.rb:38:in
`parse'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/liquid-2.3.0/lib/liquid/document.rb:5:in
`initialize'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/liquid-2.3.0/lib/liquid/template.rb:58:in
`new'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/liquid-2.3.0/lib/liquid/template.rb:58:in
`parse'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/liquid-2.3.0/lib/liquid/template.rb:46:in
`parse'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/jekyll-0.12.0/lib/jekyll/convertible.rb:79:in
`do_layout'
/home/wolf/prgm/octopress/plugins/post_filters.rb:167:in `do_layout'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/jekyll-0.12.0/lib/jekyll/post.rb:195:in
`render'
/home/wolf/.rvm/gems/ruby-1.9.3-p125/gems/jekyll-0.12.0/lib/jekyll/site.rb:200:in
`block in render'
```


どうもコードをコピーして来た時に、{% raw %} `{{ }}` {% endraw %} をくっつけて書いたのが気にくわないらしい。

そのため、octopressでは二重中カッコを書く時は対応方法がふたつあるらしい

- 1) 2つの中カッコの間にスペースを入れる `{ {`
- 2) {% raw %}{% raw %} `{{` {% endraw %}{% endraw %} のようにで文字列を囲む


