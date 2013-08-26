---
layout: post
title: "Django チュートリアル[ビュー作成]"
date: 2013-09-02 00:00
omments: false
published: true
author: Shinichi Morimoto
categories: django python tutorial
---

こんにちは。Catchball21　技術部の森本です。

DjangoによるWebアプリ開発のチュートリアルも今回が3回目になります．  
本日はビューの作成についてご説明します．

## MTVモデル

ビューの説明に移る前に，DjangoのMTVモデルについてご紹介します．  
MTVモデルはおよそ下記のようなイメージになります．

<MTVモデルの図を貼り付け>

Djangoにおいて，ユーザからリクエストを受け取った際の処理の流れは下記になります．

1. URLディスパッチにより，リクエストされたURLパターンにマッチしたView関数を呼び出す．
2. フォームからGETやPOSTでデータを受け取った際は適宜Formオブジェクトを呼び出し，validationを行う．
3. Modelを通して，DBからデータを取得する．
4. 必要なテンプレートを呼び出し，レスポンスを返す．

今回説明するViewの最低限の役割としては下記になります．

* リクエストを受け取る．
* レスポンスを返す．

## Formの作成

Viewの作成に移る前に，まずはFormオブジェクトを作成しましょう．

Webアプリケーションでの悩みどころの一つとして，validationの扱いがあります．  
DjangoではこのようなForm処理を簡易に扱う為にフォーム処理ライブラリが標準で付属されています．

フォーム処理ライブラリでは，validation以外にも下記の機能を持っています．

>
>1. フォームウィジェットから、 HTML フォームを自動的に生成して表示できます。
>2. 提出されたデータに対して、バリデーション規則 (validation rule) を適用できま す。
>3. バリデーションエラーを検出したときに、フォームをエラーメッセージ付きで表示で きます。
>4. 提出されたデータを、適切な Python のデータ型に変換できます。
>  
>[フォームの操作 — Django 1.4 documentation](http://docs.djangoproject.jp/en/latest/topics/forms/index.html#topics-forms-index)

ではFormオブジェクトを作成しましょう．Formオブジェクトはforms.pyで定義します．  
`myproject/apps/simpleboard/`ディレクトリに`forms.py`を作成し，下記を記載して下さい．

{% codeblock forms.py lang:python %}

from django import forms

from .models import Board

class BoardForm(forms.ModelForm):

	class Meta:
		model = Board

{% endcodeblock %}

上記ではBoardFormクラスの中でmodelとして，Boardモデルを指定しています．
たったこれだけのコードだけで，Model（Boardモデル）からひもづけて，Boardモデルに必要なFormオブジェクトを定義することができます．  

では，続いてViewの作成に移りましょう．


## Viewの作成

DjangoのViewは実際にはResponseオブジェクトを返すPythonの関数になります．  
と言っても，何のことだか分からないので，実際に作成してみましょう．

ちなみにDjangoでは1.3からより汎用的なクラスベースのViewが導入されています．  
今回は関数ベースのViewを使用しますが，機会があれば，また別途クラスベースのViewについてもご説明したいと思います．

Viewはviews.pyの中で定義します．  
`myproject/apps/simpleboard/views.py`を開いて，下記のように書き換えて下さい．


{% codeblock views.py lang:python %}

class Board(models.Model):

	entry = models.CharFiled(max_length=255)

{% endcodeblock %}

