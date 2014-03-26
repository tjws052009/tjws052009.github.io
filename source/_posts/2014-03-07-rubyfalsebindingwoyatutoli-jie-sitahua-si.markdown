---
layout: post
title: "rubyのbindingをやっと理解した話し"
date: 2014-03-07 18:58:12 +0900
comments: true
categories: ruby erb
---


自分で ERB を使ってて binding を良くわからず使ってたので、自分用にメモ

```ruby
#-*- coding:utf-8 -*-
#
# Ruby skeleton file
#
require 'erb'

class Huga
  def self.get_binding
    binding
  end

  def initialize(&block)
    hoge = 222
    @hoge = 444
    res = instance_eval(&block)

    puts res
  end
end


erb = ERB.new(File.read('test.txt'))

hoge = 111
@hoge = 333

test = Huga.new do
  puts "erb.result"
  erb.result
end

test2 = Huga.new do
  puts "erb.result(binding)"
  erb.result(binding)
end
```

```ruby
<%= hoge %>
<%= @hoge %>
```

上のコードを実行すると、bindingによる変数の受け渡しがどう動くか見れるかと思います。

instance_eval にローカル変数の `hoge` と、インスタンス変数の `@hoge` の受け渡しが色々と違う。当たり前だけど、こういう事ねと初めて理解した。

binding を明示的に実行すると呼び出されるクラス下のスコープを参照するようにします。
Hugaクラスで `hoge` を定義しても、そちらの `hoge` は見ないよう。
Hugaクラスの `@hoge` は、さすがに、 instance_eval で反映された状態で出る。

```
erb.result
111
333

erb.result(binding)
111
444
```
