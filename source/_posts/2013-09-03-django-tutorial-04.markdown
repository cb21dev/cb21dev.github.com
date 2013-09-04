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

{% codeblock index.html lang:html %}

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

{% endcodeblock %}

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

{% codeblock index.html lang:python %}

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
Formオブジェクトをテンプレートに渡した場合、テンプレート中でHTMLのフォームとして展開します。  
querysetはイテレータブルなオブジェクトなので、`index.html`では{% raw %}`{% for ... %}`{% endraw %}タグを使用して、格納されているオブジェクトを取り出しています。  

最後に[BootStrap](http://127.0.0.1:8000/)で少しレイアウトを整えた`index.html`を記載します。

{% codeblock index.html lang:html %}

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>SimpleBoard</title>
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">

    <!-- Optional theme -->
    <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap-theme.min.css">

    <!-- Latest compiled and minified JavaScript -->
    <script src="//code.jquery.com/jquery.js"></script>
    <script src="//netdna.bootstrapcdn.com/bootstrap/3.0.0/js/bootstrap.min.js"></script>
  </head>
  <body>

    <nav class="navbar navbar-default" role="navigation">
    <!-- Brand and toggle get grouped for better mobile display -->
      <div class="navbar-header">
        <a class="navbar-brand" href="#">SimpleBoard</a>
      </div>
    </nav>

    <div class="row">
      <div class="col-md-4"></div>
      <div class="col-md-4">
        <form class="form-horizontal" role="form" action="/" method="post">
          {% raw %}
          {% csrf_token %}
          {% endraw %}

          <div class="form-group">
            <div class="col-lg-10">
              {% raw %}
              {{ form.entry }}
              {% endraw %}
            </div>
          </div>
          <div class="form-group">
            <div class="col-lg-offset-2 col-lg-10">
              <button type="submit" class="btn btn-default">登録</button>
            </div>
          </div>
        </form>

        {% raw %}
        {% for entry in entries %}
          <div class="panel panel-default">
            <div class="panel-body">
              {{ entry.entry }}
            </div>
          </div>
        {% endfor %}
        {% endraw %}
      </div>
      <div class="col-md-4"></div>
    </div>
  </body>
</html>
{% endcodeblock %}

`python manage.py runserver`で開発サーバを起動して、[http://127.0.0.1:8000](http://127.0.0.1:8000/)にアクセスすると下記のような画面が表示されると思います。  
< Simple Board の画像を貼り付け>

## 最後に

いささか駆け足でDjangoによるWebアプリ開発についてご紹介してきましたが、いかがでしたでしょうか。  
DjangoによるWebアプリ開発の利便性を少しでも感じて頂ければ幸いです。  

今回、使用したサンプルの完全版は以下から参照することができます。  
<simpleboardをpushして、githubのurlを貼り付け>

また時間の都合上、今回のチュートリアルでご説明できなかったTopicsは下記になります。  
参考になりそうな参照先を記載していますので、お時間があるときにでもご確認頂ければと思います。  

* QuerySetのAPI
	* [QuerySet API リファレンス — Django 1.4 documentation](http://docs.djangoproject.jp/ja/latest/ref/models/querysets.html)
* 汎用view
	* [汎用ビュー — Django 1.4 documentation](http://docs.djangoproject.jp/ja/latest/topics/generic-views.html)
	* [Django Class-Based-View Inspector -- Classy CBV](http://ccbv.co.uk/)
* Djangoのテスト環境
	* [Djangoアプリケーションのテスト — Django 1.4 documentation](http://docs.djangoproject.jp/ja/latest/topics/testing.html)
* southによるDB Migration
	* [South ドキュメント — South v0.7 documentation](http://ae35.bitbucket.org/south-doc-ja/index.html)
