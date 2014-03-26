---
layout: post
title: "FactoryGirlでハッシュを保存する時"
date: 2014-02-21 15:26:17 +0900
comments: true
categories: Ruby FactoryGirl
---

理解不足だからかもしれないですが、FactoryGirlでオブジェクトを作る時に Hash
を値に使おうとした時に、怒られた。

```
factory :obj, class: Model::Object do
  obj_attr1 1
  obj_attr2 'ほげ'
  obj_hash_str {:spot1 => 0, :spot2 => 0, :spot3 => 0, :spot4 => 0}.to_json
end
```

下記のように、カッコに囲んで書けばいいみたい。なぜにかちょっとわからないんですが、ブロックとして処理しようとしてたんですかね？

```
factory :obj, class: Model::Object do
  obj_attr1 1
  obj_attr2 'ほげ'
  obj_hash_str ({:spot1 => 0, :spot2 => 0, :spot3 => 0, :spot4 => 0}).to_json
end
```

