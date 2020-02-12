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
3. javascriptの設定

## Google Cloud Platformの登録

GCPでGoogle Map APIが使用できるようにします。前提として、以下のものが準備してある想定です。

* プロジェクトの作成
* お支払いの設定

![Google Cloud Platform](/img/screenshot-2020-02-12-17.09.02.png "プロジェクトの作成と支払い設定")

詳細は割愛しますが、お支払いの設定は済ませておくとあとで地図表示の際にエラーにならなくて済みますっ

それから今回は地図を表示するだけじゃなくて、

* 地図の色を変える
* 準備したピンを使う

ので、細かい設定ができるようGoogle Maps Javascrit APIを使用します。この時点で16種類ほど地図のAPIがありましたが、`Maps Javascript API` だけ有効にすればOKです。

![Maps Javascript API](/img/screenshot-2020-02-12-17.09.02.png "Maps Javascript API")

これを押して、有効にしたら、次はAPIキーです。
APIキーは、使用する地図が誰がサイトに埋め込んでいて、かつ地図が誰か知らない人から不正に利用ができないよう保護するような意味合いで、**必須**となります。これがないとこんな表示になります。

![Google Maps API エラー画面](/img/screenshot-2020-02-12-17.18.27.png "Google Maps API エラー画面")

<br>

## Google Map用API Keyの発行

サイドメニューの`APIとサービス`、そして`認証情報`と進むとこのような画面が表示されますので、「＋認証情報を作成」ボタンから新規にキーを発行します。

![認証情報を作成](/img/screenshot-2020-02-12-17.21.47.png "認証情報を作成")

<br>

そうすると自動でAPIキーが発行されます。

![APIキーの作成](/img/screenshot-2020-02-12-17.34.10.png "APIキーの作成")

ここにあるように本番環境でこのキーは見ることができてしまうので、このソースをコピーすれば誰でもこの地図を利用することができるようになってしまいます。ですので\`キーの制限\`を行うことをおすすめします。

制限の方法としては、

* ドメイン名でのしばり
* 使用できるAPIでのしばり（ここだとMaps Javascript API）

です。

<br>

## javascriptの設定（webpack）

最初に地図に色をつけた状態を確認したいので、ここから地図サンプルを試します。\
https://mapstyle.withgoogle.com/

![google maps platform styling wizard](/img/screenshot-2020-02-12-17.43.52.png "google maps platform styling wizard")

<br>

このウィザードはすべての設定を調整できるので、項目が非常に多いです。ですので、今回は良くあるパターンとして、グレーにするという設定をしたいと思います。

![地図をグレーにする](/img/screenshot-2020-02-12-17.54.32.png "地図をグレーにする")

<br>

とは言っても、設定はサイドメニューの`Silver`を選択するだけです。それとなぜだか分からないけどピンの表示がデフォルトでオフになっているので、左下の`MORE OPTION`から

* Feature type: All
* Element type: Icon
* Stylers: inherit

と選んでいくと、ピンが表示されるようになります。
これでFINISHボタンを押すと、スタイルフォーマットのjsonもしくはURLを発行してくれます。
そして今回使うのはjsonの方ですので、上の方になります。

![Google Map カラーリング json](/img/screenshot-2020-02-12-18.00.53.png "Google Map カラーリング json")

<br>

このソースをサイトへ埋め込みます。
まずはこちらのオフィシャルサイトでサンプルコードを参考にしましょう。

地図を埋め込む初期設定:
[https://developers.google.com/maps/documentation/javascript/tutorial?hl=ja](https://developers.google.com/maps/documentation/javascript/adding-a-google-map?hl=ja)

自前のピンの設定:
[https://developers.google.com/maps/documentation/javascript/adding-a-google-map?hl=ja](https://developers.google.com/maps/documentation/javascript/adding-a-google-map?hl=ja)

これを元に以下のソースを作成しました。

google-map.js
```
export default function googleMap () {
    let map = {}
    const icon = {
        url: '/画像まで/のパスを/入力/pointer.png',
        scaledSize: new google.maps.Size(50, 50)
    }
    function initMap () {
        map = new google.maps.Map(document.getElementById("google-map"), {
            center: { lat: 35.809159, lng: 137.243678 },
            zoom: 17,
            styles:　＜ここにさっきのjsonを入れる＞
        });
        new google.maps.Marker({
            position: new google.maps.LatLng(35.809159, 137.243678),
            icon: icon,
            map: map
        })
    }
    initMap()
}
```
ピンのサイズは今は縦横50%で設定していますが、ここは適宜変更してください。

このままだと、`initMap()`のスコープがMAP APIのコールバックの時に通らないので、このjsをインポートするところで、
```
import googleMap from './google-map';
window.initMap = googleMap;
```

これで大概の地図表示はカバーできるんじゃないかと思いますっ。  
ぜひご参考ください。
