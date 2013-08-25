---
layout: post
title: "Django チュートリアル[プロジェクト作成-モデル作成]"
date: 2013-08-26 00:00
comments: false
published: true
author: Shinichi Morimoto
categories: django python tutorial
---

こんにちは。Catchball21　技術部の森本です。

蒸し暑い日が続いていますが，体調等は崩されていないでしょうか？  
僕は少し夏バテ気味なので，早く涼しくなって欲しいと祈るばかりです．

今回は前回に続いて、DjangoによるWebアプリ開発についてご紹介します.  
プロジェクト・アプリ・モデルの各作成について詳しく解説していきます.

## 作成するWebアプリ

今回のチュートリアルでは、シンプルな掲示板を題材にしています.  
完成イメージは下記のようなイメージになります．

{% img /images/20130826/20130826_1.png 'SimpleBoard image' 'this is a SimpleBoard image' %}

フォームから入力し，入力した内容を下部に表示する単純なCRUDを行うWebアプリですね．

## プロジェクト作成

まずはプロジェクトを作成しましょう．
Djangoをインストールすると`django-admin.py`コマンドが併せてインストールされます．
プロジェクト作成は`django-admin.py`を使用して，作成します．

	(django_tutorial)% django-admin.py startproject myproject

上記コマンドを実行すると`myproject`という名前のディレクトリが作成されます．
以後はこの`myproject`内で作業をしていくのですが，その前にプロジェクトについてDjangoの便利な機能をご紹介します．

Djangoでは1.4から`project template`という機能が追加されました．  
`project template`ではフォルダ構成や追加した3rd Party のライブラリ等をテンプレート化し，纏めておくことができます．またそれらのテンプレートを使用してプロジェクトを作成することができます．  
つまりプロジェクトを通じて得たノウハウをテンプレート化し，次回のプロジェクトでも簡単に使用することができる機能と言えます．  
テンプレートはgithub等で公開されているものがあるので，それらを使用することで初心者でもDjangoのベストプラクティスを利用することができます．

`project template`については，hirokikyさんが素晴らしい[紹介記事](http://d.hatena.ne.jp/hirokiky/20120702/1341231182)を書いてらっしゃるので、皆さんご覧になってみてください。

今回は`django-skel`というテンプレートを使用したいと思います．  
先程作成した`myproject`ディレクトリを削除して，下記コマンドを実行し，新たに`django-skel`テンプレートを利用するプロジェクトを作成しましょう．

	(django_tutorial)% django-admin.py startproject --template=https://github.com/rdegges/django-skel/zipball/master myproject

`myproject`ディレクトリが作成されていれば，成功です．

`myproject`ディレクトリに移動し，必要なライブラリをインストールします．`django-skell`では，`reqs`ディレクトリ以下に開発環境(`dev.txt`)と本番環境(`prod.txt`)で使用するライブラリを分けて記載しています．まずは開発用のライブラリをインストールするだけで良いでしょう．

	(django_tutorial)% pip install -r reqs/dev.txt



## アプリ作成

プロジェクトが作成できたら，アプリを作成します．
django-skelではアプリの配置場所として`myproject/apps`が用意されている為，`myproject/apps`配下に作成します．

	(django_tutorial)% cd myproject/apps
	(django_tutorial)% python ../../manage.py startapp simpleboard

`myproject/apps`配下に`simpleboard`という名前のディレクトリが作成されていることを確認して下さい．
また`simpleboard`配下にはviewとmodelの雛形が作成されます．

	(django_tutorial)% ls simpleboard
	__init__.py models.py   tests.py    views.py

アプリを作成したら，Djangoに新規作成したアプリを登録する必要があります．  
`myproject/settings/common.py`を編集して先程作成した`simpleboard`を登録しましょう．  
`LOCAL_APPS `に下記のように`simpleboard`を追記します．  

{% codeblock common.py lang:python %}

LOCAL_APPS = (
    'apps.simpleboard',
)
{% endcodeblock %}


## モデル作成

アプリの作成まで完了したら，いよいよモデルを作っていきましょう．  
まずは今回必要となるモデルについて考えましょう．  
今回作成するのはシンプルな一行掲示板の為，最低限入力した内容を保存するカラムだけがあれば問題なさそうです．
SQLで表すとは下記のような感じになるでしょうか．

{% codeblock models.sql lang:sql %}
CREATE TABLE "simpleboard_board" (
   "id" integer NOT NULL PRIMARY KEY,
   "entry" varchar(255) NOT NULL
);

{% endcodeblock %}  

ではモデルを作成しましょう．  
Djangoでは`models.py`がモデルに対応する為，`models.py`を編集します．

{% codeblock models.py lang:python %}
from django.db import models


class Board(models.Model):

	entry = models.CharFiled(max_length=255)


{% endcodeblock %}

上記では`entry`というフィールドを持った`Board`モデルを定義しています．
クラスがDBのテーブル，クラスのアトリビュートがDBのカラムに対応しています．

モデルが定義できたら，DBに反映する必要があります．
プロジェクトのトップディレクトリに戻り下記コマンドを実行して下さい．

	(django_tutorial)% python manage.py syncdb
	Syncing...
	Creating tables ...
	Creating table auth_permission
	Creating table auth_group_permissions
	Creating table auth_group
	Creating table auth_user_groups
	Creating table auth_user_user_permissions
	Creating table auth_user
	Creating table django_content_type
	Creating table django_session
	Creating table django_site
	Creating table django_admin_log
	Creating table south_migrationhistory
	Creating table simpleboard_board

	You just installed Django's auth system, which means you don't have any superusers defined.
	Would you like to create one now? (yes/no): yes
	Username (leave blank to use '****): admin
	Email address: admin@*****
	Password:
	Password (again):
	Superuser created successfully.
	Installing custom SQL ...
	Installing indexes ...
	Installed 0 object(s) from 0 fixture(s)

	Synced:
	 > django.contrib.auth
	 > django.contrib.contenttypes
	 > django.contrib.sessions
	 > django.contrib.sites
	 > django.contrib.messages
	 > django.contrib.staticfiles
	 > django.contrib.humanize
	 > django.contrib.admin
	 > django.contrib.admindocs
	 > south
	 > compressor
	 > apps.simpleboard
	 > debug_toolbar

	Not synced (use migrations):
	 - djcelery
	(use ./manage.py migrate to migrate these)

初めて`syncdb`を実行した際は，管理者ユーザを作成するかを聞かれます．  
`yes`と入力すると，管理者ユーザに必要な情報（ユーザ名，メールアドレス，パスワード）を聞かれるので，適当なものを入力して下さい．
作成した管理者ユーザは`admin site`へログインする際に使用します．

以上でモデルの作成は完了です．

次回はviewの作成についてご紹介する予定です．