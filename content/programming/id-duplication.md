---
title: "HTMLでid属性の重複を防ぐ"
description: ""
date: "2022-11-20T10:47:33+09:00"
thumbnail: 
  src: "img/thumb/html.jpg"
  visibility:
    -list
categories:
  - "programming"
tags:
  - "python"
menu:
main:
  name: プログラミング
  weight: 2
---
HTMLタグのid属性は、同じページ内では重複してはいけないのですが、どうしても重複してしまう場合。

環境：python

<!--more-->

以前、forloop.counter やオブジェクトの id(uuid4) を使って重複を防いでいましたが、それでも稀な状況で重複が出現することがあります。

[for ループで id が重複するとき](https://progress-advance.com/programming/how2django1/)

今回 python の random を使用して、id を以下のように記載して重複を防ぐことに成功しました。

`views.py`
```
import random

number = random.randint(1,10000)

```

`template`
```
<div id="test_{{ number }}_{{ forloop.counter }}_{{ object.id }}"> test </div>
```

randomモジュールは乱数を生成するほか、リストやタプル、文字列から要素を抽出したり、一様分布、正規分布（ガウス分布）、対数正規分布などを計算することができるようです。

[Pythonのrandomで乱数を作ってみよう！ choice、sample、randintから応用編まで](https://camp.trainocate.co.jp/magazine/python-random/)

[random — Generate pseudo-random numbers](https://docs.python.org/3/library/random.html)

[python random number between 1 and 100](https://pythonspot.com/random-numbers/)

##### お薦め

[RAPT有料記事140(2017年1月23日)悟ってこそ成長し、成功する。](https://rapt-neo.com/?p=41873)

[
  RAPT有料記事430(2019年12月23日）主と自分の向かう方向が一致し、かつ時が一致したとき、主から豊かに霊感と閃きを受けて、やることなすこと全てがうまくいくようになる。
](https://rapt-neo.com/?p=52143)


{{<x user="Rapt_plusalpha" id="1593948062589689856">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1593941227736829952">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1593937153578655744">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1593930275733766146">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1593916913360326656">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1593560210601168896">}}
&nbsp;
