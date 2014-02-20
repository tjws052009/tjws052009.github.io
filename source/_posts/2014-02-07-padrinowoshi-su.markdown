---
layout: post
title: "padrinoを試す"
date: 2014-02-07 14:15:44 +0900
comments: true
categories: ruby padrino
---

ちょっと試しに padrino を使ってみようかと。

```
$ gem install padrino

$ padrino g project hottmp -d activerecord -a mysql -s jquery -e erb -c compass -t rspec
```

とりあえずコレで指定した物が入りました。

個人的にあんまり Haml　とかのテンプレートエンジンが好きでなくて、erb
あたりがちょうどしっくりきます。Coffee より jQuery だったり。

これってどうなんでしょうね？

とりあえず、もうちょっといじって、それっぽくなってきたら続報あげます。
