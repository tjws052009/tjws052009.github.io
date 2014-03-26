---
layout: post
title: ssh の unknown host の回避
date: 2014-02-20 12:30:20 +0900
comments: true
categories: 
---

# unknown hosts を自動で authorize したい

以下の設定で、いちいち unknown host を authorize するために yes/no
を答えなくていい。

```
# ~/.ssh/configの該当するHost に
StrictHostKeyChecking no
```


正直言ってあんまり安全では無いことは承知でやるなら上で良さそう。

もしもうちょっとちゃんとやるなら、下記のページみたいな物が必要かもしれないですね。

[Add SSH Known Hosts with Chef](https://sethvargo.com/add-ssh-known-hosts-with-chef/)
