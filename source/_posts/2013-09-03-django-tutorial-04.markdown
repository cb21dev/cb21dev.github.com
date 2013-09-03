---
layout: post
title: "Django チュートリアル[テンプレート]"
date: 2013-09-03 22:51
comments: false
published: false
author: Shinichi Morimoto
categories: django python tutorial
---

こんにちは。Catchball21　技術部の森本です。

本日はTemplate(テンプレート)についてご説明します。  
DjangoによるWebアプリ開発のチュートリアルは今回で最後になります。  

## Template(テンプレート)

まずは前回の最後で作成した`index.html`について復習しましょう。

```html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>SimpleBoard</title>
  </head>
  <body>
    <form action="/" method="post">
    {% raw %}
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="登録">
    </form>

    {% for entry in entries %}

    <p>{{ entry.entry }}</p>

    {% endfor %}
    {% endraw %}
  </body>
</html>

```

基本的にはhtmlそのものですが、{% raw %}`{{ entry.entry }}`{% endraw %}や{% raw %}`{% for entry in entries %}`{% endraw %}等、いくつか見慣れない構文がみられますね。 

Djangoのテンプレートでは`変数`や`タグ`を使用することができます。  
上記では{% raw %}`{{ entry.entry }}`{% endraw %}が`変数`、{% raw %}`{% for entry in entries %}`、 `{% csrf_token %}`{% endraw %}が`タグ`にあたります。

テンプレート中で変数は{% raw %}`{{ spam }}`{% endraw %}と記載します。  
コンテキストで`spam: 1`として渡した場合、テンプレート中の`{{ spam }}`は`1`に置き換えられます。

`タグ`はテンプレート内で何らかの処理を行いたいときに使用します。  
テンプレート中で`タグ`は{% raw %}`{% egg %}`{% endraw %}と記載されます。  
例えば、テンプレート中で分岐処理を行いときは、{% raw %}`{% if ... %}`{% endraw %}タグ、ループ処理を行いたい時は{% raw %}`{% for ... %}`{% endraw %}タグ等が標準で用意されています。  
またタグは自分で定義することもできます。

前回作成したindex viewでテンプレートに渡されたコンテキストを確認しましょう。

{% codeblock views.py lang:python %}

    entries = Board.objects.all()
    form = BoardForm()
    return render_to_response(
        'index.html',
        context_instance=RequestContext(
            request,
            {
                'form': form,
                'entries': entries
            }
        )
    )

{% endcodeblock %}

上記ではコンテキストとして、`form`変数にはBoardFormオブジェクト、`entries`変数にはBoardモデルのquerysetが渡されています。


