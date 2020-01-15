---
templateKey: blog-post
title: webpackでデプロイの時にfaviconを自動で追加する方法
date: 2020-01-13T03:18:28.447Z
description: vue-cliではfaviconのタスクが含まれていませんでしたので、設定方法をまとめました。
featuredpost: true
featuredimage: /img/apps-blur-button-close-up-267350.jpg
tags:
  - vue-cli
  - vue
  - webpack
  - favicon
  - favicons-webpack-plugin
  - CopyWebpackPlugin
---
### faviconジェネレーターサイトを利用する方法
まずはあんまりスマートじゃありませんが、アイコン生成サイトを利用する方法を紹介します。手順としては以下の通り。

1. faviconをPNGで作成
2. faviconジェネレータサイトを利用
3. webpackのタスクへ追加

<br/>

##### 1. faviconをPNGで作成
これはできた素晴らしいサイトができていると思うので、簡単にできると思いますっ。
サイズは`512 x 512`で作るとiphoneのアイコンなどでもキレイに仕上がります。

##### 2. faviconジェネレータサイトを利用
たくさん生成サイトはありますが、私はこちらを利用しました。bookmarkに登録されているので、反応で使ってしまいます。  
このサイトは、微調整が画面で確認しながらできるのがいいですね。

RealFaviconGenerator  
[https://realfavicongenerator.net/](https://realfavicongenerator.net/)

##### 3. webpackのタスクへ追加
ジェネレータでアイコンができました。zipされているので、解凍するとファイルが10ファイルぐらいできます。これをビルドした時に自動で公開用のディレクトリ`/dist`に入るようしします。まずは、
```
新たにfaviconsディレクトリを作成し、解凍したファイルをコピー
```

<br>

そして、デプロイ時に自動で`/dist`にコピーされて欲しいので、`webpack.prod.conf.js`の`CopyWebpackPlugin`へ新たにアイコンをコピーする記述を追加します。

```javascript
...
    // copy custom static assets
    new CopyWebpackPlugin([
      {
        from: path.resolve(__dirname, '../static'),
        to: config.build.assetsSubDirectory,
        ignore: ['.*']
      },
      // ここから
      {
        from: path.resolve(__dirname, '../favicons'),
        to: './',
        ignore: ['.*']
      }
      // ここまで
    ])
...
```
`to: './'`と固定でディレクトリ指定していて、全ての`favicons`ディレクトリにあるファイルは`dist`のルートにコピーしますというコマンドになります。

以上ですっ。簡単っ！あとは、ビルド（`npm run build`）して`/dist`ディレクトリにアイコンがコピーされているはずっ


### プラグイン favicons-webpack-pluginを使った方法
アイコンを制作する以外全て自動化できます。最初に紹介したジェネレータサイトなんて使わなくても簡単にできてしまいます。そのwebpackプラグインが`favicons-webpack-plugin`です。

favicons-webpack-plugin  
[https://github.com/jantimon/favicons-webpack-plugin](favicons-webpack-plugin)

### まとめ
結局使ったのは最初のジェネレータサイトを利用した方法で作業しました。
どうしても画面で確認しながら、持っていないwindowsでの表示の仕方などを画面で確認しながらできるのはやっぱり頼りになります。
それに、レアなケースですが、アイコンのデザインがかなり細い線が含まれるものだったので、faviconなど小さいサイズのアイコンには線を太くしたもの、大きいiphoneなどのアイコンには線の細いものを使用するなど、細かな調整が必要だったのもこちらを採用した理由です。


##### 参考にしたサイト：
[https://stackoverflow.com/questions/37298215/add-favicon-with-react-and-webpack](https://stackoverflow.com/questions/37298215/add-favicon-with-react-and-webpack)

