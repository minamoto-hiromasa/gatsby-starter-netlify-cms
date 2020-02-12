---
templateKey: blog-post
title: Google Mapの埋め込み
date: 2020-02-12T07:39:44.618Z
description: Google Map API が課金制になったけど、実際は一定量を超えなければ課金されないので、Google Cloud Platformから設定してみた
featuredpost: true
featuredimage: /img/adventure-city-country-destination-240834.jpg
tags:
  - google map
  - javascript
  - google cloud platform
---
コンテンツは以下の内容です

1. Google Cloud Platformの登録
2. Google Map用API Keyの発行
3. javascriptの設定（webpack）

## Google Cloud Platformの登録

GCPでGoogle Map APIが使用できるようにします。前提として、以下のものが準備してある想定です。

* プロジェクトの作成
* お支払いの設定

![](/img/screenshot-2020-02-12-16.57.38.png)
詳細は割愛しますが、
