---
title: "【Pandas】備忘録-その1-"
description: ""
date: "2022-08-21T07:50:31+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "pandas"
menu:
main:
  name: プログラミング
  weight: 2
---
以前、pandas の使い方を勉強しましたが、使っていないと忘れてしまうので備忘録を書くことにしました。

環境：jupyter notebook

<!--more-->

```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
%matplotlib inline
```
データの読み込み。データは以前、インストラクターから頂いたものです。

```
train= pd.read_csv("C:file/to/train.csv")

train
```
![data](/img/pandas/data.png)
&nbsp;

データの先頭行を見る。
```
train.head(n=1)
```
データの最終行を見る。
```
train.tail(n=1)
```
行数と列数を確認したい場合:shapeは関数ではないのでカッコは要らない。
```
train.shape
```
out:(207, 12)
```
train.shape[0]
```
out:207
```
train.shape[1]
```
out:12

基本統計量の確認

```
train.describe()
```
![data2](/img/pandas/data1.png)

&nbsp;
データの型の確認

```
train.info()
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 207 entries, 0 to 206
Data columns (total 12 columns):
|#|     Column  |       Non-Null Count|  Dtype  |
|---|  ------       |  -------------- | -----  |
 0  | datetime      | 207  non-null  |  object |
 1  | y             | 207  non-null  |  int64  |
 2  | week          | 207  non-null  |  object |
 3  | soldout       | 207  non-null  |  int64  |
 4  | name          | 207  non-null  |  object |
 5  | kcal          | 166  non-null  |  float64|
 6  | remarks       | 21  non-null   |  object |
 7  | event         | 14  non-null   |  object |
 8  | payday        | 10  non-null   |  float64|
 9  | weather       | 207  non-null  |  object |
 10 | precipitation | 207  non-null  |  object |
 11 | temperature   | 207  non-null  |  float64|

dtypes: float64(3), int64(2), object(7)
memory usage: 19.5+ KB

一つのカラムに注目
例:"y"

```
train['y']

（二つのカラムを選択する場合は、train[['y','soldout']]）
```
out:
||y|
| -- | -- |
0    |   90|
1    |  101|
2    |  118|
3    |  120|
4    |  130|
     | ... |
202  |   59|
203  |   50|
204  |   45|
205  |   56|
206  |   40|

Name: y, Length: 207, dtype: int64

yの平均と中央値を見る。

・平均値はmean関数、中央値はmedian関数を使う

```
train['y'].mean()
```
out : 86.6231884057971
```
train['y'].median()
```
out : 78.0

yの値が150以上のデータを見る(平均値以上を見る)。

```
train[train['y'] > 150]

（train[train['y'] > train['y'].mean()]）
```
out:

|      datetim|  y| week|  soldout |        name| kcal|remarks| event|  payday |
|-|-|-|-|-|-|-|-|-|
8  | 2013-11-28|  151 |   木 |   0|        ハンバーグ   |NaN |    NaN |  NaN |    NaN|   
10 |  2013-12-2|  151 |   月 |   1|        マーボ豆腐   |NaN |    NaN |  NaN |    NaN|   
11 |  2013-12-3|  153 |   火 |   1|     厚揚げ豚生姜炒め   |NaN |    NaN |  NaN |    NaN|   
12 |  2013-12-4|  151 |   水 |   1| クリームチーズ入りメンチ   |NaN |    NaN |  NaN |    NaN|   
13 |  2013-12-5|  171 |   木 |   0|  鶏のカッシュナッツ炒め   |NaN |    NaN |  NaN |    NaN|   
15 |  2013-12-9|  165 |   月 |   1|   ハンバーグデミソース   |NaN |    NaN |  NaN |    NaN|   
16 | 2013-12-10|  155 |   火 |   0|やわらかロースのサムジョン   |NaN |    NaN |  NaN |    1.0|   
17 | 2013-12-11|  157 |   水 |   0|         五目御飯   |NaN |    NaN |  NaN |    NaN|   
20 | 2013-12-16|  160 |   月 |   0|    カキフライタルタル   |NaN |    NaN |  NaN |    NaN|   
23 | 2013-12-19|  151 |   木 |   0|      ポーク味噌焼き   |NaN |    NaN |  NaN |    NaN|  

曜日が"月"となっているデータのみを見る。

```
train[train['week'] =='月']
```

|    |  datetime |   y |week|  soldout  |           name  | kcal| remarks | 
|-|-|-|-|-|-|-|-|
0    |2013-11-18 |  90 |   月|        0  |       厚切りイカフライ  |  NaN|     NaN |  
5    |2013-11-25 | 135 |   月|        1  |           鶏の唐揚  |  NaN|     NaN |  
10   | 2013-12-2 | 151 |   月|        1  |          マーボ豆腐  |  NaN|     NaN |  
15   | 2013-12-9 | 165 |   月|        1  |     ハンバーグデミソース  |  NaN|     NaN |  
20   |2013-12-16 | 160 |   月|        0  |      カキフライタルタル  |  NaN|     NaN |  
 -||||||||
177  | 2014-8-18 |  56 |   月|        0 |     洋食屋さんのメンチカツ | 396.0 |    NaN |  
182  | 2014-8-25 |  55 |   月|        0 |        白身魚の南部焼き | 412.0 |    NaN |  
187  |  2014-9-1 |  65 |   月|        1 |         ビーフシチュー | 380.0 |    NaN |  
192  |  2014-9-8 |  68 |   月|        1 |         鶏肉の山賊焼き | 385.0 |    NaN |  
201  | 2014-9-22 |  29 |   月|        0 |             筑前煮 | 395.0 |    NaN |  
205  | 2014-9-29 |  56 |   月|        1 |        豚肉と玉子の炒め | 404.0 |    NaN | 


曜日が"火"となっているデータを"y"で昇順・降順にする。
* ソートはsort_values関数を使います。sort_values(by="XXX")と書く。
* 降順にしたい場合はカッコの中にオプションとしてascending=Falseを書く。
```
train[train['week'] =='火'].sort_values(by='y',ascending=False)
```

曜日が月曜日の時のyの平均値を見る。
```
train[train['week'] == '月'].mean()
```
曜日が月曜日の時のyの平均値。
```
train[train['week'] == '月'].mean()
```
y&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;94.051282

soldout&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0.487179

kcal&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;398.970588

remarks&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NaN

payday&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.000000

temperature&nbsp;&nbsp;&nbsp;19.656410

dtype: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;float64

```
train[train['week'] == '月']['y'].mean()
```

out: 94.05128205128206

次回に続きます。

##### お薦め

[国連が掲げる「SDGs（持続可能な開発目標）」はカール・マルクスの「共産主義宣言」の丸写しだった!!　国際機関を乗っ取り、世界を共産主義に染めていく中国共産党](https://rapt-plusalpha.com/51652/)

[東京都が発表した「外国人起業支援事業」にパソナが関与　外国人を優遇した融資制度に対し、東京都に抗議の電話が殺到](https://rapt-plusalpha.com/51636/)

[コロナワクチンの副反応を発症し、運動や日常生活ができなくなる子供が続出 海外では「小児認知症」と診断される子供たちが急増](https://rapt-plusalpha.com/51624/)

[中国のスパイ「小池百合子」都知事が、4回目のコロナワクチン接種の対象にエッセンシャルワーカー（警察、消防、教員、物流業者等）を加えるよう政府に要望　都民を殺戮し、さらなる外国人受け入れを画策](https://rapt-plusalpha.com/51616/)

[【イギリス】数千の企業が「脱中国依存」を目指す　次期首相有力候補のスナク前財務相もイギリス国内の全ての孔子学院を閉鎖すると宣言](https://rapt-plusalpha.com/51594/)
&nbsp;

{{<youtube gNaZBg3jl9g>}}
&nbsp;
{{<youtube ebyUeIGqgds>}}
&nbsp;
 
{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/Mina%E3%83%A9%E3%82%B8%E3%82%AA2/06bafba746f29b1106b31819bc48cba1e41690f1?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}