---
layout: post
title: "get and post method for sinatra"
date: 2014-02-04 18:17:18 +0900
comments: true
categories: sinatra ruby get post
---

Sinatraでエンドポイントを用意して、リアルタイムにIDの重複チェックとかしたいと思い、ajaxで使用する前提の物を書いてみた。

id の値をサーバーにPOSTし、結果が json で返ってくるようにしてみた。
今回は開発環境でドメインまたいでテストを行えたらいいなと思い
jsonpで返そうと当初は考えていた。

```
# 頭の中ではこんな感じになればいいなと
$ curl -d 'id=testtsugi01' http://test.tsugi2009.com/verify_id
#  => verify({"status": "ok", error: []})
```

で、こんなコードを書いてみた

```
# sinatra.rb
post '/verify_id' do
  result = User.verify_id(params['id'])

  {% raw %}return "verify(#{ {status: result, error: []}.to_json})"{% endraw %}
end
```

```javascript
# test.js
<form action="http://test.tsugi2009.com/verify_id" method="POST" id="piyof">
  <input type="text" name="id" />
  <input type="submit" />
</form>

<script>
  $('#piyof_submit').on('click', function() {
    $.ajax({
      type: "POST",
      url: "http://test.tsugi2009.com/verify_id",
      data: $('#piyof').serialize(),
      dataType: 'jsonp',
      jsonpCallback: 'verify',
      success: function(data) {
        console.debug(data)
      }
    })
  });
</script>
```

とりあえず、 http://test.tsugi2009.com/ 下なら大丈夫だけど http://tsugi2009.com/ とかドメインまたぐとちゃんと動かない。
POSTのはずが、GETでパラメーターを送信してたり。（それで 404 が返ってきて一瞬焦った）

良く考えると（良く考えなくても）、そもそもクロスドメイン制約でjsonpのPOSTとかできへんやん・・・
しかも、POST前提に組んでたので、GETになったjsonp リクエストには対応出来ず 404 が返る状況に。

とりあえず、色々諦めて、開発中はjsonpを使って、本番ではjsonにしてもらうっていう綱渡りような事に。
ただし、SinatraでひとつのエンドポイントをGETとPOSTに対応するため、似たようなコードを二回書く事になるのか、と思いGETとPOSTに対応した物が出来ないか調べてみた。そしてここに行き着く。

[Accepting Both Get And Post Method in Sinatra](http://ujihisa.blogspot.jp/2009/11/accepting-both-get-and-post-method-in.html)

出来るならやるしないですよね。言うことで、それも含め実装。


```
# sinatra.rb
module Sinatra
  module GetOrPost
    def get_or_post(path, opts={}, &block)
      get(path, opts={}, &block) 
      post(path, opts={}, &block) 
    end
  end
end

register GetOrPost

get_or_post '/verify_id' do
  result = User.verify_id(params['id'])

  # 本当に正しくPOSTされたら json っていう前提で書いてます。gkbr
  {% raw %}
  return {status: result, error: []}.to_json if request.request_method == 'POST'
  return "verify(#{ {status: result, error: []}.to_json})"
  {% endraw %}
end

```

```javascript
# prod.js
// 本番用 同じ、 tsugi2009.comの下で動いてるはず
<form action="http://tsugi2009.com/verify_id" method="POST" id="piyof">
  <input type="text" name="id" />
  <input type="submit" />
</form>

<script>
  $('#piyof_submit').on('click', function() {
    $.ajax({
      type: "POST",
      url: "http://tsugi2009.com/verify_id",
      data: $('#piyof').serialize(),
      dataType: 'json',
      success: function(data) {
        console.debug(data)
      }
    })
  });
</script>

# test.js
// テスト用。 test.tsugi2009.com から叩かれるとは限らない
<form action="http://test.tsugi2009.com/verify_id" method="POST" id="piyof">
  <input type="text" name="id" />
  <input type="submit" />
</form>

<script>
  $('#piyof_submit').on('click', function() {
    $.ajax({
      type: "GET",
      url: "http://test.tsugi2009.com/verify_id",
      data: $('#piyof').serialize(),
      dataType: 'jsonp',
      jsonpCallback: 'verify',
      success: function(data) {
        console.debug(data)
      }
    })
  });
</script>
```

やっと動いた。ajaxで色々と忘れそうなので、この実装をどうにかしたいと思ってるけど、どうしよう。
