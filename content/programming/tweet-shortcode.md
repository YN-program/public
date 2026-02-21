---
title: "【HUGO】twitter のショートコードの修正"
description: ""
date: "2022-10-17T12:32:08+09:00"
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
Hugo のtwitter のショートコードについて、"The "tweet" shortcode will soon require two named parameters: user and id." というメッセージが出るようになりました。

config.toml の`baseurl`や`title`が書いてあるところに、ignoreErrors = ["error-remote-getjson"]を追記すれば、取り敢えずエラーは出なくなりました。

{{<x user="kz_morita" id="1485082588968796163">}}

以上。

##### お薦め

{{<x user="Rapt_plusalpha" id="1581643042078871554">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1581620620697038848">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1581603744755896321">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1581580343584645120">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1581238517887545344">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1580891111065030656">}}
&nbsp;
