---
templateKey: blog-post
title: CanonicalをRequest Headersへ設定
date: 2020-01-09T07:23:22.540Z
description: headerタグではなく、HTTPヘッダーへcanonicalを埋め込む設定です
featuredpost: true
featuredimage: /img/book-on-red-surface-3519355.jpg
tags:
  - canonical
  - request headers
  - wordpress
  - php
---
### canonicalヘッダーへの埋め込み方

まずはPHPでの埋め込み方です

```
header('Link: <https://www.pluve.work/>; rel="canonical"');
```

この書き方で完了です。すでにheaderタグ内に`rel="https://www.pluve.work"`を設定している場合は、これを削除しておきましょうっ。それとwordpressなどでAll In One SEOやYoastなどのSEOプラグインでcanonicalを付加するような設定などが生きている場合もあるかもなので、ブラウザのdeveloper toolなどで、削除・設定後、確認しましょう。

ヘッダーのレスポンスを確認するとこんな感じで確認ができます。

![]()

canonicalの目的
canonicalの設定いろいろ
同一なページってどの程度同一かは、判断の仕方が気になる
プラグインの公開
