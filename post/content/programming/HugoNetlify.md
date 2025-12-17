---
title: "Hugo + Netlify + Github によるブログ構築"
description: ""
date: "2022-04-21T18:29:19+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Hugo"
  - "Netlify"
menu:
main:
  name: プログラミング
  weight: 2
---
静的サイトジェネレーター Hugo と Netlify, GitHub を使って、ブログを構築。

いろんなサイトを見ると簡単にできそうに書いてありましたが、色々と設定で苦戦したので、取り敢えず上手くいった方法を記載します。

<!--more-->
##### Hugoのインストールから設定まで

これについては、以下のサイトに詳しく書いてありますので、その通りにすれば初めてでもできます。

[Hugo でのブログの作り方・使い方（Windows） 初心者向け手順解説](https://blog-dai.com/hugo-windows-install-howto/)

なお、Hugo のインストール後に環境変数の PATH の設定をしますが、設定が確実に反映されるようにパソコンを再起動したほうが良さそうです。

[Windows 10 環境変数の設定と反映する方法](https://www.tipsfound.com/windows10/11011)

また、Hugo のテーマが沢山公開されていますが、Hugo のバージョンアップにより動かないテーマもあるそうですから、updated が最近の日付になっているテーマを選ぶほうが良さそうです。

[【HUGO】最新版をインストールして、サイトを作り、テーマを当ててビルドするまで](https://noauto-nolife.com/post/hugo-install-latest/)


##### GitHubアカウント作成

Netlify と GitHu を連携させるので、GitHub のアカウントを持っていない場合は、先に作成しておきます。

[GitHubアカウントの作成方法 (2021年版)](https://qiita.com/ayatokura/items/9eabb7ae20752e6dc79d)

##### Netlify と GitHub の連携・設定

以下のサイトにその方法が書いてあります。
ムームードメインの DNS を使用する場合は、こちら。

[Netlifyと静的サイトジェネレーターHUGOで1ヶ月約100円でブログ運営をする方法【独自ドメイン使用】](https://noauto-nolife.com/post/startup-netlify/)

Netlify の DNS を使用する場合は、こちらに手順が書いてあります（Xserver使用）。

[Netlifyにカスタムドメインを適用する手順](https://ysmlog.net/posts/blog-domain-exchange/)

Netlify の設定途中で、次のような画面に出くわします。

![netlify2](/img/netlify2.jpg)

Hugo でブログを作るときに、コマンドプロンプト（windows）で`hugo`というコマンドを実行すると、public という名前のフォルダが作られますが（[Hugo でのブログの作り方・使い方（Windows） 初心者向け手順解説](https://blog-dai.com/hugo-windows-install-howto/)）、GitHub に public フォルダをアップロードするのか、中身だけをアップロードするのかで、設定内容が変わります。

この Publish directory に関しては、public の中身だけをアップロードする場合は、何も記載しなくても良いと思いますが、public のフォルダをアップロードした場合は、以下のように「public」と記載する必要があると思います(試していないので多分です)。

![netlify1](/img/netlify.png)

私は GitHub に public の中身だけをアップロードしたので、Publish directory に public と記載すると、「public がない」とエラーが出ました。

` Failed during stage 'building site': Deploy directory 'public' does not exist`

また、自分のパソコンで`hugo`コマンドを実行し、公開用ファイルを生成して 、GitHub にアップロードするのか、GitHub にソースファイルを置いて、Netlify で`hugo`コマンドを実行するのかで、Build Command に「hugo」と記載するかどうかが変わってくると思います。
ここでも、私は public の中身だけをアップロードしていたからか、エラーが出まくりました。

`Failing build: Failed to build site`

`Failed during stage 'building site': Build script returned non-zero exit code: 1 `

結局、Publish directory と Build Command には何も書かないことで、Netlify と GitHub の設定が上手くいきました。

[Hugo + Github + Netlifyで簡単サイト・ブログ作成（GitHubで全て管理）](https://www.tech-ek.com/post/hugo-manage-in-github-netlify/)

後、公開記事が一つもない場合も、Netlify と GitHub の連携に失敗すると、どこかのサイトに書いてありました。

##### 記事の書き方

これも最初に紹介したサイトに記載されていますが、マークダウン記法で記事を書きます。

[Hugo でのブログの作り方・使い方（Windows） 初心者向け手順解説](https://blog-dai.com/hugo-windows-install-howto/)

[Markdown記法/書き方（見出し・表・リンク・画像・文字色など）](https://notepm.jp/help/how-to-markdown)

ただ、マークダウン記法の一部が反映されないテーマがあったので、テーマを選んだら試しに一つ記事を書いてみたほうが良いかもしれません。

##### マークダウンエディタ

[【2022年版】Markdown(マークダウン)エディタ厳選まとめ＜Win/Mac/iOS/Android＞](https://notepm.jp/blog/724)

以下の動画を参考にして、マークダウンでの記事の記載から GitHub へのアップロードまでが簡単にできる Visual Studio Code を使うことにしました。

{{<youtube hdpMw3hyQq4>}}

&nbsp;
 
Hugo の`config.toml`の設定など、他にもすることはありますが、取り敢えずこれで記事の作成から公開までできると思います。

[Hugoでブログ構築＆Mainroadテーマを適用](https://ysmlog.net/posts/mainroad-init/)