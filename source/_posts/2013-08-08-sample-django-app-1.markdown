---
layout: post
title: "Django チュートリアル [準備編]"
date: 2013-08-08 23:24
comments: true
categories: django python tutorial
---

こんにちは。Catchball 21　技術部の森本です。

これから何回かに分けて、DjangoによるWebアプリ開発について紹介しようと思います。  
今回は準備編になります。

## Djangoって？

と、その前にご存知無い方の為にDjangoについて簡単に紹介させて頂きます。

DjangoはPythonで書かれたオープンソースのWebアプリケーションフレームワークで、元々はニュース系のサイトを管理する目的で開発されたそうです。
いわゆるオールインワン型のフレームワークで特徴として、MVCパターンによく似た、MTVパターン（Model - Template - View）に従って開発することや標準でリッチな管理インターフェイスを持つこと等が挙げられます。

[{% img /images/0806/django_web_site.png 'Django Web Site' 'this is a django web site image' %}](https://www.djangoproject.com/)

弊社でも受託開発や自社サービス開発にDjangoを利用しています。

## 開発環境

今回のチュートリアルでは下記の環境を想定しています。

* Mac OS X 10.7
* Python 2.7
* Django 1.5

## 準備

では、早速準備に移って行きましょう。  
まずはPythonの仮想環境を構築することができるvirtualenvを導入しましょう．

virtualenvはPythonのパッケージ管理ツールであるpipからインストールすることができます．

    % pip install virtualenv

virtualenvのラッパーで仮想環境を管理しやすくするvirtualenvwrapperも導入しましょう．
こちらもpipでインストールことができます．

    % pip install virtualenvwrapper

virtualenv, virtualenvwrapperを使うためにはshellに設定が必要になります．  
zshを使用している場合は`.zshrc`に以下の行を追加します．(bashの場合も同じように`.bashrc`に記述すれば大丈夫だと思います.)

{% gist id=b9901e9b2b96073d5cd7 %}

virtualenvwrapperの導入まで完了したら，仮想環境を作成しましょう．  
今回は仮想環境としてdjango_tutorialを使用します

    % mkvirtualenv django_tutorial
    New python executable in django_tutorial/bin/python
    Installing setuptools............done.
    Installing pip...............done.

shellのプロンプトの先頭に下記のように仮想環境名が表示されていれば，成功です．

    (django_tutorial)%

virtualenvwrapperについて簡単に使い方を紹介しておきます．

    (django_tutorial)% deactivate #deactivateで仮想環境から抜けることができます．
    % workon #workonで作成した仮想環境の一覧を見ることができます．
    django_tutorial
    % workon django_tutorial # workon <仮想環境名> で仮想環境に入ることができます．
    (django_tutorial)% mkvirtualenv hogehoge # mkvirtualenv <仮想環境名> で仮想環境を作成することができます．
    New python executable in hogehoge/bin/python
    Installing setuptools............done.
    Installing pip...............done.
    (hogehoge)% deactivate
    (hogehoge)% rmvirtualenv hogehoge # rmvirtualenv <仮想環境名>で指定した仮想環境を削除することができます．
    Removing hogehoge...

仮想環境の作成まで完了したら，Djangoをインストールします．
こちらもpipからインストールすることができます．

    (django_tutorial)% pip install django

以上で準備は完了です！  


次回はDjangoのプロジェクト作成からモデルの作成までを紹介させて頂く予定です．
