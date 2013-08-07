---
layout: post
title: "Django チュートリアル [準備編]"
date: 2013-08-06 23:24
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
とは言っても
