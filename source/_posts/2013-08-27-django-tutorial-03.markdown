---
layout: post
title: "Django チュートリアル[view(ビュー)作成]"
date: 2013-09-02 00:00
omments: false
published: false
author: Shinichi Morimoto
categories: django python tutorial
---

こんにちは。Catchball21　技術部の森本です。

DjangoによるWebアプリ開発のチュートリアルも今回が3回目になります。  
本日はview(ビュー)の作成についてご説明します。  

## MTVモデル

ビューの説明に移る前に、DjangoのMTVモデルについてご紹介します。  
MTVモデルはおよそ下記のようなイメージになります。  

<MTVモデルの図を貼り付け>

Djangoにおいて、ユーザからリクエストを受け取った際の処理の流れは下記の通りです。

1. URLディスパッチにより、リクエストされたURLパターンにマッチしたview関数を呼び出す。
2. フォームからGETやPOSTでデータを受け取った際は適宜Formオブジェクトを呼び出し、validationを行う。
3. Modelを通して，DBからデータを取得する。
4. 必要なテンプレートを呼び出し、レスポンスを返す。

今回説明するviewの最低限の役割は、下記になります。

* リクエストを受け取る。
* レスポンスを返す。

## Formの作成

viewの作成に移る前に、まずはFormオブジェクトを作成しましょう。

Webアプリケーションでの悩みどころの一つとして、validationの扱いがあります。  
DjangoではこのようなForm処理を簡易に扱う為にフォーム処理ライブラリが標準で付属されています。

フォーム処理ライブラリは、validation以外にも下記の機能を持っています。

>
>1. フォームウィジェットから、 HTML フォームを自動的に生成して表示できます。
>2. 提出されたデータに対して、バリデーション規則 (validation rule) を適用できます。
>3. バリデーションエラーを検出したときに、フォームをエラーメッセージ付きで表示できます。
>4. 提出されたデータを、適切な Python のデータ型に変換できます。
>  
>[フォームの操作 — Django 1.4 documentation](http://docs.djangoproject.jp/en/latest/topics/forms/index.html#topics-forms-index)

ではFormオブジェクトを作成しましょう。Formオブジェクトはforms.pyで定義します。  
`myproject/apps/simpleboard/`ディレクトリに`forms.py`を作成し、下記を記載して下さい。

{% codeblock forms.py lang:python %}

from django import forms

from .models import Board

class BoardForm(forms.ModelForm):

	class Meta:
		model = Board

{% endcodeblock %}

上記ではBoardFormクラスの中でmodelとして、Boardモデルを指定しています。  
たったこれだけのコードで、Model（Boardモデル）から紐付けて、Boardモデルに必要なFormオブジェクトを定義することができます。

では、続いてviewの作成に移りましょう。


## viewの作成

Djangoのviewは実際にはResponseオブジェクトを返すPythonの関数になります。  
と言っても、何のことか伝わりずらいので、実際に作成してみましょう。  

ちなみにDjangoでは1.3から、より汎用的なクラスベースのviewが導入されています。  
今回は関数ベースのviewを使用しますが、機会があれば、また別途クラスベースのviewについても解説したいと思います。  

viewはviews.pyの中で定義します。  
`myproject/apps/simpleboard/views.py`を開いて、下記のように書き換えて下さい。


{% codeblock views.py lang:python %}

from django.shortcuts import redirect
from django.shortcuts import render_to_response
from django.shortcuts import redirect
from django.template import RequestContext

from .models import Board
from .forms import BoardForm


def index(request):

    if request.method == "POST":
        form = BoardForm(request.POST)
        if form.is_valid():
            entry = form.cleaned_data["entry"]
            print entry
            Board.objects.create(
                entry=entry
            )
        return redirect('/')

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

viewの処理の流れは、概ね下記の内容です。

1. リクエストのメソッドによって、処理を振り分け
2. それぞれの処理で必要であれば、モデルからデータを取得
3. テンプレートを呼び出し、レスポンスを返す

上記の`views.py`の例では、必要なモジュールのインポート定義の後、`def index(request):`からがviewの定義になります。
リクエストを受け取ってからの処理は下記の通りです。

1. リクエストのメソッドがPOSTかどうかをチェックします。(12行目)
2. POSTデータをFormオブジェクトに渡し、validationのチェックをかけます(13, 14行目)
3. validationが通っていたら、受け取ったデータをBoardに登録し、トップにリダイレクトするレスポンスを返します。(15-20行目)
4. Boardモデルから全てのデータを取得します。(22行目)
5. Formオブジェクトを生成します。(23行目)
6. テンプレート(`index.html`)を呼び出し、レスポンスを返します。またテンプレート内で変数のように使用できるコンテキストもこの時に指定します。(24-33行目)

テンプレートとして、`index.html`を指定していますが、まだ作成はしていないですよね。
`myproject/templates/index.html`を作成して、下記を記載してください。


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

テンプレートの詳細については、次回ご紹介させて頂きます。

最後にURLとviewとをマッピングさせる必要があります。  
URLとviewとのマッピングは`myproject/urls.py`に記載します。  
下記のように記載して下さい。

{% codeblock urls.py lang:python %}

from django.contrib import admin
from django.conf.urls import patterns, include, url

from apps.simpleboard.views import index


# See: https://docs.djangoproject.com/en/dev/ref/contrib/admin/#hooking-adminsite-instances-into-your-urlconf
admin.autodiscover()


# See: https://docs.djangoproject.com/en/dev/topics/http/urls/
urlpatterns = patterns('',
    # Admin panel and documentation:
    url(r'^admin/doc/', include('django.contrib.admindocs.urls')),
    url(r'^admin/', include(admin.site.urls)),
    url(r'^$', index)
)

{% endcodeblock %}

url関数の一つ目の引数にurlのパターン、２つ目の引数にそのurlにマッピングさせたいviewを指定します。
上記の場合はドメイン名（例：http://localhost)にアクセスした際にindexのviewを返すようマッピングしています。

では開発サーバ上でSimpleBoardにアクセスしてみましょう。  
プロジェクトのトップディレクトリで`python manage.py runserver`を実行して下さい。


    (django_tutorial)% python manage.py runserver
    Validating models...

    0 errors found
    August 28, 2013 - 19:27:34
    Django version 1.5.2, using settings 'myproject.settings.dev'
    Development server is running at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.

8000番ポートで開発サーバが起動されるので、[http://127.0.0.1:8000](http://127.0.0.1:8000/)にアクセスして下さい。  
下記のような画面が表示されれば、成功です。

<SimbleBoard実行画面を貼り付け>


view(ビュー)の作成については以上です。  
次回はTemplateについてご説明する予定です。