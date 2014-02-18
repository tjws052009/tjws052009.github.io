---
layout: post
title: "My first post"
date: 2014-02-03 12:01:37 +0900
comments: true
categories: git
---

The `git log` command has a couple of useful flags, but I never can remember what they are.

Therefore, I decided to use the git aliases to create an alias command which
would include the flags on default.

```
# ~/.gitconfig

[alias]
  lg = 'log --decorate'
  
```

By adding the lines above, `git lg` would be identical to calling `git log
--drecorate`.

Now the only problem is if I would remember to actually use the alias...
