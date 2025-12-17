---
title: "ModuleNotFoundError:No module named '' "
description: ""
date: "2022-06-23T07:27:06+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  
menu:
main:
  name: プログラミング
  weight: 2
---
`ModuleNotFoundError: No module named '***.editor'; '***' is not a package`というエラーの原因。

<!--more-->

`moviepy.py`

```
from moviepy.editor import *

(中略)
```

実行すると、`ModuleNotFoundError: No module named 'moviepy.editor'; 'moviepy' is not a package`というエラーが出る。

>実行するファイルをmoviepy.pyという名前で保存すると、import moviepy文でインポートされるモジュールはそのファイル自身になってしまいます。
>
>標準ライブラリやインストールしたモジュールと同じ名前でファイルを作ってはダメです。
>
>https://teratail.com/questions/obmg1ixvhtaach

試しに使ってみようとして、適当にファイル名を付けて失敗。二度目の失敗なので、戒めとして記事にしました。

その他、`ModuleNotFoundError: No module named 'moviepy'`というエラーは、movie.py をインストールしたPythonとIDLEを動かしているPythonが異なる場合に起こるとのこと。

[pythonでIDLEを使ってエラーが出てしまう。](https://teratail.com/questions/358172)

##### 参考記事
[〇 pythonでimport時に、ModuleNotFoundError: No module namedが出た時の対処手順](https://qiita.com/ymto/items/e00e95543aab2d4d45ee)

##### お薦め記事

[〇 【コロナワクチン接種の結果】2022年1月〜3月の死亡者数が過去4年間の平均値より全都道府県で3%〜20.1%も増加](https://rapt-plusalpha.com/45698/)

[〇 イルミナティは、霊界に天国と地獄が存在し、死後も天国で永遠に幸せに暮らせる方法があることを隠蔽してきた（十二弟子・エリカさんの証）](https://rapt-plusalpha.com/45402/)

[〇 RAPTブログが、この世のほとんど全ての宗教が悪魔崇拝であると暴き、その中でも本当の神様を信じて愛することができるように導いてくださった!!（十二弟子・NANAさんの証）](https://rapt-plusalpha.com/45199/)

[〇 イルミナティが人口削減について会議している映像がネット上に流出](https://rapt-plusalpha.com/14135/)

[〇 「警告!! 日本は既に中国共産党に乗っ取られている」](https://rapt-plusalpha.com/34533/)