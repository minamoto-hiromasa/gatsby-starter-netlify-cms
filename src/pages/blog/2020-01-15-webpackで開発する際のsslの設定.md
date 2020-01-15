---
templateKey: blog-post
title: webpackを使ってローカルで開発するときのSSLの設定
date: 2020-01-15T06:59:10.717Z
description: mkcertを利用してlocalhostに自己著名証明書を設定し、webpackで有効にする方法を紹介します
featuredpost: true
featuredimage: /img/brass-ornate-vintage-key-on-black-computer-keyboard-39389.jpg
tags:
  - mkcert
  - webpack
  - webpack-dev-server
  - 自己著名証明書
  - SSL
---
サーバのSSL設定はレンタルサーバなどにすでに設定されているか、Let's encryptなどを利用して証明書を簡単に設定できます。

<br>

### 自己著名証明書の作成
はじめにmkcertを利用して、自己著名証明書を発行します。
（macでの作業を前提としています。windowsはすいません…）

** mkcertプラグイン **  
[https://github.com/FiloSottile/mkcert](https://github.com/FiloSottile/mkcert)

インストール方法：
```
MacBook-Pro ~ % mkcert -install
```

サイトでは色々なURLやワイルドカード、IPなどを設定していますが、私は `localhost` のみを設定しました。
```
MacBook-Pro ~ % mkcert localhost
```
すると、
- localhost.pem
- localhost-key.pem

が生成されます。同時に、MacBookの `~/Library/Application Suport/mkcert/` に以下のファイルが生成されます。

- rootCA-key.pem
- rootCA.pem

以上で、`mkcert`の設定はここまでです。

<br>

### webpackの設定
`webpack.config.js`ファイルを編集します。設定方法はこちらを参考にしました。 

**webpack-dev-serverのオプション設定**
[https://webpack.js.org/configuration/dev-server/#devserverhttps](https://webpack.js.org/configuration/dev-server/#devserverhttps)

この中で`devServer`オプションに以下のように追加するだけです。

```
...
devServer: {
    https: {
        key: fs.readFileSync('./localhost.pem'),
        cert: fs.readFileSync('./localhost.pem'),
        ca: fs.readFileSync('~/Library/Application Support/mkcert/rootCA.pem')
    }
}
...
```

これで`webpack-dev-server`のサービスを立ち上げると、自動的にhttpがhttpsへリダイレクトするようになります。


### ローカルのSSL問題
設定方法を説明しましたが、実際に導入してみて、色々と問題がありました。  
`localhost`がすべてSSL対応されるので、他のプロジェクトでSSLソケットを使えるようdevServerを設定していないプロジェクトは見れなくなってしまうことにっ。全部のプロジェクトを設定し直さなくちゃいけないです。

また、スマホもSSL対応となるので、ホームネットワーク内でIP指定して検証作業などを行うときもSSL対応がされないので、見れなくなりますっ。

### あんまりローカルでSSLをする利点が少ないかも…
このローカルのSSL問題はかなりネックで、もう止めようかと思ってます。
もしローカルでSSLを設定する利点などありましたら教えてくださいっ
