---
title: "【HUGO】Odyseeの動画埋め込み"
description: ""
date: "2022-08-18T05:46:57+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Hugo"
menu:
main:
  name: プログラミング
  weight: 2
---
ショートコードを作り、Odyseeの動画をブログに埋め込む方法。

<!--more-->

layouts/shortcodesの中にodysee.htmlを作成。

```
.
└── layouts
    └── shortcodes
        ├── odysee.html
```

`odysee.html`の中はこれだけ。

```
{{.Inner}}
```

そして、｛｛＜odysee＞｝｝｛｛＜/odysee＞｝｝の間に、共有したい動画の埋め込みコードをコピーして貼り付けるだけ。

`｛｛＜odysee＞｝｝<iframe id="odysee-iframe" width="560" height="315" src="" allowfullscreen></iframe>｛｛＜/odysee＞｝｝`

閃いてやってみたら上手くいきました。


{{<file>}}<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/01/20971499ac41177428aba9520a5309d4eaaa5750?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/04/a1fa0266a675a42f6c3026fc0e127e1e615669ce?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/Mina%E3%83%A9%E3%82%B8%E3%82%AA2/06bafba746f29b1106b31819bc48cba1e41690f1?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
