---
title: "【Netlify】HUGOのバージョンが古くて（?）デプロイに失敗"
description: ""
date: "2023-03-24T14:44:52+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "HUGO"
menu:
main:
  name: プログラミング
  weight: 2
---
NetlifyとHUGOを使用してブログを記載していますが、新しい記事が反映されなくなったので、原因の究明と解決に至るまで。

<!--more-->

Netlifyにログインし、「Deploys」タブをクリックし、Deploy logを確認したところ、次にようなエラーが出ていました。

```
 Error during Hugo 0.95 install: error downloading project gohugoio/hugo, from https://github.com/gohugoio/hugo/releases/download/v0.95/hugo_extended_0.95_Linux-64bit.tar.gz - binary doesn't seem to exist: 404
```

HUGOのバージョン0.95がインストールできませんと。

で、「Deploys」→「Deploy settings」→「Build & deploy」の「Environment」→「Go to Environment variables settings」へと進み、HUGOのバージョンを0.95から最新のもの(v0.111.3)に変更し、「Deploys」に戻ってエラーになっている「Deploy log」をクリックし、Retryすることで、無事に解決しました。

https://github.com/gohugoio/hugo/releases

振り返ってみると簡単な事でしたが、今まで問題なく動いていたものが突然動かなくなったので、解決に手間取ってしまいました。

同じような状況になって困っている人のお役に立てたら幸いです。

##### お薦め

{{<x user="Rapt_plusalpha" id="1638884266871488512">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1638882795065061377">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1638876066818445312">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1638874327818059781">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1638858805604921344">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1638844252284424193">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1638841881231753217">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1638487089766604800">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1638133188341170176">}}
&nbsp;