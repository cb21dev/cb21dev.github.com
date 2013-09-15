---
layout: post
title: "Django のユーザー認証を二段階認証に対応する [ 準備編 ]"
date: 2013-09-16 00:00
comments: false
published: true
author: Umeda Takefumi
categories: django UmedaTakefumi two-factor 
---

こんにちは、Catchball21 技術部の梅田です。

これから何回かに分けて、Django を使い、2段階認証に対応したアプリケーションの作成方法をご紹介したいと思います。今回は準備編です。


## 2段階認証とは

皆さんWebサービスを利用する際には、ユーザーIDとパスワードを利用していると思います。2段階認証とは、ユーザーIDとパスワード以外に新たにもう1つ要素を追加して、認証方式を強化するシステムのことです。追加する要素として下記のアイテムが主流になっています。

 * USBトークン
 * スマートカード
 * バイオメトリクス（指紋認証,静脈認証,網膜認証,声紋認証）
 * ワンタイムパスワード

## 2段階認証の必要性

近年、特定のWebサービスが何者かに攻撃されIDとパスワードが盗まれるという事件が頻発しています。そういった行為に対処する方法の1つとして2段階認証が注目され始めています。
日本では数年前から、オンラインバンキングやMMORPGなどで利用されていますが、他分野でも導入の検討や、新たに導入した企業が複数あるようです。

## 主要なWebサービスは、2段階認証を提供している

主要なWebサービスは、2段階認証を提供しています。以下がそのリストになります。

* Google
  * [Google 2段階認証プロセス](http://www.google.co.jp/intl/ja/landing/2step/ "Google 2段階認証プロセス")
* Apple ID
  * [Apple ID：Apple ID の 2 段階認証についてよくお問い合わせいただく質問 (FAQ)](http://support.apple.com/kb/HT5570?viewlocale=ja_JP&locale=ja_JP "Apple ID：Apple ID の 2 段階認証についてよくお問い合わせいただく質問 (FAQ)")
* EVERNOTE
  * [2 段階認証](https://www.evernote.com/shard/s184/sh/6406df73-45b4-4534-b601-d9635ffb223a/ecc94311e4eb5d155420fd40c5f3984b "2 段階認証")
* Microsoft
  * [2 段階認証: FAQ - Microsoft Windows](http://windows.microsoft.com/ja-jp/windows/two-step-verification-faq "2 段階認証: FAQ - Microsoft Windows")
* Dropbox
  * [Dropbox - アカウントで 2 段階認証を有効にするには。](https://www.dropbox.com/help/363/ja "Dropbox - アカウントで 2 段階認証を有効にするには。")
* amazon web service
  * [Multi-Factor Authentication | アマゾン ウェブ サービス（AWS 日本語）](http://aws.amazon.com/jp/mfa/ "Multi-Factor Authentication | アマゾン ウェブ サービス（AWS 日本語）")


## Djangoで2段階認証を実装するには

今回はワンタイムパスワードを利用し、2段階認証を実装する方法をご紹介します。
Djangoでの認証の実装方法は、いくつかあります。
最も簡単な方法は Bouke Haarsma さんが作成した django-two-factor-auth というモジュールを利用する方法です。

* Bouke/django-two-factor-auth · GitHub
  * [https://github.com/Bouke/django-two-factor-auth](https://github.com/Bouke/django-two-factor-auth)

次回は、このモジュールを利用し Django で2段階認証をコーディングする方法をご紹介したいと思います。

