---
title: "【HUGO】Shortcode で文字色、背景の変更、動画の埋め込み"
description: ""
date: "2022-08-10T10:19:43+09:00"
thumbnail: ""
  # src: ""
  # visibility: 
  #   - list
categories:
  - "programming"
tags:
  - "Hugo"
menu:
main:
  name: プログラミング
  weight: 2
---

ショートコードを作り、HUGOの記事の文字色や背景の変更、動画を埋め込む方法。

<!--more-->

layouts/shortcodesの中にhtmlファイルを作成。
```
.
└── layouts
    └── shortcodes
        ├── file.html
        ├── magenta.html
        └── bgyellow.html

```

{{<magenta>}}文字をマゼンタ色に{{</magenta>}}
&nbsp;

`magenta.html`

```
<span style="color:magenta;">{{.Inner}}</span>

```
そして、このように｛｛＜magenta＞｝｝文字を囲みます。｛｛＜/magenta＞｝｝（注：実際には半角）


{{<bgyellow>}}背景を黄色に{{</bgyellow>}}
&nbsp;

`bgyellow.html`
```
<span style="background-color:yellow;">{{.Inner}}</span>
```

動画の埋め込み。｛｛＜file ファイル名＞｝｝のように記載します。

{{<file2 final>}}
&nbsp;

`file.html`

```
<video width=100% controls>
    <source src="/img/video/{{ index .Params 0 }}.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>
```

動画ファイルは、static/img/video の中に保存しておきます。

```
.
└── static
    └──  img
          └── video
```

もしくは、Cloudinaryなどのサービスを使用します。

ただし、無料利用枠を超えないように注意が必要です。

[Cloudinaryの新しいプランを調べてみた](https://qiita.com/kanaxx/items/b3366025e6715562d8f9)

[Netlifyの無料枠を越えてしまい、1日20ドル請求されてしまった話](https://blog.tomoya.dev/posts/i-was-billed-beyond-limits-of-netlify/)

##### 参考記事

[ショートコードで本文内に HTML スニペットを埋め込む](https://maku77.github.io/hugo/shortcode/basic.html)

[Hugoの記事にコンテンツを埋め込む](https://foresuke.com/post/hugo_embed/)

##### お薦め記事

[【有害アプリ・TikTok】「失神ゲーム」に参加した子供の死亡事故が世界中で相次ぐ　過激なダイエット動画の大量送信で、若者の摂食障害を助長](https://rapt-plusalpha.com/50737/)

[【岸田内閣改造人事】中国共産党のスパイ「河野太郎」がデジタル担当大臣に抜擢され批判殺到　一時、Twitterで「デマ太郎」がトレンド入り](https://rapt-plusalpha.com/50725/)

[【中国共産党の脱退者が4億人に達する】米Twitter社前CEOの「ジャック・ドーシー」が「中国共産党を終わらせよ」とツイート](https://rapt-plusalpha.com/50762/)

[林芳正外務大臣が中国のハニートラップにかけられ、スパイ行為に加担している疑いが浮上!!　スマホのカメラで常時盗撮、機密情報を漏洩させている可能性大](https://rapt-plusalpha.com/50692/)