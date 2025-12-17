---
title: "【Django】INSTALLED_APPS の適切な書き方"
description: ""
date: "2022-06-21T08:30:49+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Django"
menu:
main:
  name: プログラミング
  weight: 2
---
DjangoのINSTALLED_APPSは、最初に記載されているアプリケーションが優先されるため、記載する順番が重要なのだそうです。
<!--more-->

正しい書き方。上から順に、
* ローカルアプリケーション
* サードパーティアプリケーション
* Djangoアプリケーション


```
INSTALLED_APPS = [ 
    #Local apps
    'test.apps.TestConfig',

    #Third party apps
    'rest_framework',

    #Django apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

INSTALLED_APPSは、最初にリストされているアプリケーションが優先されるので、Django管理画面の既存のテンプレートや静的ファイルをカスタマイズしたい・上書きしたいという状況の時に、Djangoアプリケーションを上位に記載していると、そちらが優先されてしまい、変更が反映されないそうです。

適切に記載することで、テンプレートや静的ファイル、マネジメントコマンドを上書きする変更が反映されるそうです。

また、`'test.apps.TestConfig',`を`'test,'`のように、直接アプリ名を書いてはいけないそうです。

これらを知ることで、将来起こりうる問題を事前に回避することが出来ました。

##### 参照記事
[〇【Django】settings.pyのINSTALLED_APPSにはどのように書くのが適切か【順番とapps】](https://noauto-nolife.com/post/django-settings-installed-apps/)

[〇 DjangoのINSTALLED_APPSの順番がめちゃくちゃ重要だった話](https://jumpyoshim.hatenablog.com/entry/order-of-installed-apps-with-django-is-important)

##### お薦め
[〇【中国製IoT機器は個人情報収集ツール】中国共産党はコーヒーメーカーを使って個人情報を収集していた!!](https://rapt-plusalpha.com/45380/)

[〇【米、数兆ドルの損失】中国共産党のハッカー集団、知的財産を世界中から盗み出す](http://rapt-plusalpha.com/42670/)

[〇 中国人スパイの「橋下徹」が“侮辱罪厳罰化”について「これは月刊花田に向けての法律やな」とツイートするも、「お前に向けての法律だよ」と国民から批判殺到](https://rapt-plusalpha.com/45592/)

[〇 NHK党・立花孝志が党首討論でまたも爆弾発言「中国人が10億円で議席買収」 中国共産党員の「山本太郎」が動揺し、1議席3億円で買収されていることをバラす](https://rapt-plusalpha.com/45575/)

[〇「参政党」とは、中国では“中国共産党を補佐する政党”を意味する　参政党とカルト統一協会の蜜月関係も明らかに](https://rapt-plusalpha.com/45512/)

[〇【閲覧注意】猛毒コロナワクチンによる凄惨な副作用の症例](http://rapt-plusalpha.com/10468/)

[〇 人間の肉体は、霊を育てるための母体だと教えてくれたRAPTブログ　人々は自分の霊を育てる方法を知らず、死後は悪霊のように永遠に彷徨わなければならない（十二弟子・KAWATAさんの証）](http://rapt-plusalpha.com/45526/)