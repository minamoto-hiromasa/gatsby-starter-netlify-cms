---
templateKey: blog-post
title: mkcertをアンインストールしたら、localhostがhttpsから戻らなくなった問題（HTST）
date: 2020-01-16T02:31:25.916Z
description: >-
  mkcertをアンインストールすると、http://localhost:8080が勝手にhttps://localhost:8080にリダイレクトしてしまう。原因はHTST（HTTP
  Strict Transport Security）
featuredpost: true
featuredimage: /img/brass-ornate-vintage-key-on-black-computer-keyboard-39389.jpg
tags:
  - HTST
  - mkcert
  - アンインストール
  - HTTPS
  - HTTP
  - リダイレクト
  - Chrome
---
先日、ローカルの開発環境でもSSLで検証して、Let's encryptも`cron`で定期的に更新して、やった感出したかったんですが、色々問題があって今回アンインストールしました。

以前の記事：


#### 問題点１

テストしているlocalhostをIP指定でモバイルでも検証できるよう開発環境を準備しているのですが、このmkcertを設定してしまうと、モバイルでもCertificationを求められるようになるので、どうするか途方に暮れてしまった。

#### 問題点２

こちらの方が影響が大きいかもしれませんが、他のプロジェクトも同様にlocalhostでのアクセスはSSL接続になってしまいます。もしSSL接続を設定するようWebpack-Dev-Serverなどに設定を施していない場合はやっぱり、接続ができなくなります。

こんな感じで、これらの障壁に比べて特に不都合があるわけでもないのにローカルで自己著名証明書を発行して、HTTPS接続を利用可能にする理由を見失ってきたので、アンインストールすることにしました。

<br>

### アンインストール

[mkcert](https://github.com/FiloSottile/mkcert)のサイトでアンインストール方法がないで紹介します。

```
MacBook-Pro ~ % mkcert -uninstall
```

これでシステムからキーファイルと、CERTファイルも削除されます。
これでアンインストールは完了しました。

ただし、実はこれだけでは終わりません。mkcertをインストールしたあと、Chromeでhttp接続するとhttpsへリダイレクトされてしまいます。

<br>

### HTTPS接続を解除（リダイレクト）する方法

まずはchromeのHSTS設定のページへアクセスします。

```
chrome://net-internals/#hsts
```

<br>

ここでまず `localhost` がHSTSに登録されているか調べます。

![](/img/screenshot-2020-01-16-12.11.21.png)

Domainに `localhost` と入力してすでに登録されてないか調べます。もしここで`dynamic_sts_domain:localhost`が表示されたら、削除をしましょう。

![](/img/screenshot-2020-01-16-12.39.35.png)

画面一番下に `Delete domain security policies` がありますので、ここに `localhost` を入力して削除します。

以上で設定は完了です！

<br>

これはブラウザにlocalhostはSSLで接続しますよと登録していることが原因です。
HSTS(HTTP Strict Transport Security)にlocalhostはいつもSSL接続で運用しますよとchromeに登録をmkcertインストール時に自動でしてしまうようです。
