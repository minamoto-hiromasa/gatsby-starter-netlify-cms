---
templateKey: blog-post
title: phpでcanonicalをResponse Headerに設定
date: 2017-01-04T15:04:10.000Z
description: 通常はheadタグに記述するケースがほとんどですが、Response Headerに記載するようにしました。
featuredpost: true
featuredimage: /img/matteo-maretto-9kkplorgouy-unsplash.jpg
tags:
  - canonical
  - Response Header
  - head tag
  - Advanced Rest Client
---
最初に実際の記載方法です。
```
header('Link: <https://google.com>; rel="canonical"');
```
phpの独特な記述方法なのかもしれませんが、canonicalに設定するURLを"Link: \<URL>;"のように記述します。それとrel="canonical"も設定するような記述です。

一般的にはhtmlの`head`タグに以下のように記載します。
```
<head>
...
<link rel=”canonical” href=”https://google.com”/>
...
<head>
```
canonicalの設定はwordpressでSEO対策として良く出てくるトピックですが、他にも.htaccessで設定することもできます。これは実際には試していないので、割愛させていただきますっ。また試してみる機会が合ったら更新しますっ。


wordpressでこのResponse Headerに登録する方法を紹介しましたが、実は副作用があるようでした。
```
link: <https://kmc-bunko.com/wp-json/>; rel="https://api.w.org/", <自分のサイトのURL>; rel=shortlink
```
wordpressは初期状態では、以上のようなLink情報が含まれています。
ここに２つ情報が含まれていて、片方はwordpress apiを利用できることを知らせる情報と、URLを短縮（分かりやすくphpで書き直す）を知らせるような情報が登録されています。

_[WordPressがheadに挿入する「api.w.org」とは。またその無効化の方法](https://hacknote.jp/archives/36229/)  
こちらで詳しい内容が紹介されています。_

ですが、一番最初に紹介したcanonicalのResponse Header登録をすると、これらが上書きされてしまいます。実際にREST APIが無効になっているか以下のようなURLで試してみました。
```
https://自分のWPサイトのドメイン/wp-json/wp/v2/posts
```
実際はこれで記事の一覧が問題なく取得できていました。

余談ですが、Response HeaderをGUIで見やすく表示できる以下のChromeプラグインがおすすめです。

[Advanced REST Client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo)  
こちらはResponse Headerも見れますが、APIのレスポンスであるjsonを整形して表示してくれるので、とても便利ですっ、ぜひ。

参考にしたページ:  
[https://www.searchenginepeople.com/blog/hwoto-canonical-headers.html](https://www.searchenginepeople.com/blog/hwoto-canonical-headers.html)
[https://moz.com/blog/how-to-advanced-relcanonical-http-headers](https://moz.com/blog/how-to-advanced-relcanonical-http-headers)
