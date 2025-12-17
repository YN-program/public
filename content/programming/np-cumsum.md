---
title: "累積和の求め方-Numpy,Pandas,itertools-"
description: ""
date: "2022-09-04T11:56:24+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "numpy"
  - "python"
menu:
main:
  name: プログラミング
  weight: 2
---
リスト型のデータから、累積和を求める方法。

環境は python で、for文 で記載するとコードがスッキリしなかったので、いい方法がないか調べてみました。

<!--more-->

データは以下のようなもの。

```
df_list = [ i for i in range(100)]
print(df_list)

[0, 1, 2, 3, 4, ... 95, 96, 97, 98, 99]
```

結果として得たいもの。

```
result = [0]
i = 0
for j in range(100):
    a = result[i] + j
    result.append(a)
    i = i + 1
del result[0]
print(result)

[0, 1, 3, 6, 10, ... 4560, 4656, 4753, 4851, 4950]
```

方法として、pandas, numpy, itertools の3通り見つかりました。

pandas と numpy では numpy のほうが速く、numpy と itertools では numpy のほうが速いという記事があるそうですが、私の環境では itertools > numpy > pandas　の順に速かったので、 最終的には itertools を採用しました。

しかし、全ての方法を記載しておこうと思います。

まず、pandas。

```
import pandas as pd

df_list = [ i for i in range(100) ]

df = pd.DataFrame(df_list).cumsum()
print(df)

[結果]
       0
0      0
1      1
2      3
3      6
4     10
..   ...
95  4560
96  4656
97  4753
98  4851
99  4950

[100 rows x 1 columns]
```
別の列に表示することも出来ます。

```
df['sum'] = pd.DataFrame(df_list).cumsum()
print(df)

[結果]
     0   sum
0    0     0
1    1     1
2    2     3
3    3     6
4    4    10
..  ..   ...
95  95  4560
96  96  4656
97  97  4753
98  98  4851
99  99  4950

[100 rows x 2 columns]
```

次に、numpy。

```
import numpy as np

df_list = [ i for i in range(100) ]

a = np.array(df_list)
b = np.cumsum(a)

[結果]
array([   0,    1,    3,    6,   10,   15,   21,   28,   36,   45,   55,
         66,   78,   91,  105,  120,  136,  153,  171,  190,  210,  231,
        253,  276,  300,  325,  351,  378,  406,  435,  465,  496,  528,
        561,  595,  630,  666,  703,  741,  780,  820,  861,  903,  946,
        990, 1035, 1081, 1128, 1176, 1225, 1275, 1326, 1378, 1431, 1485,
       1540, 1596, 1653, 1711, 1770, 1830, 1891, 1953, 2016, 2080, 2145,
       2211, 2278, 2346, 2415, 2485, 2556, 2628, 2701, 2775, 2850, 2926,
       3003, 3081, 3160, 3240, 3321, 3403, 3486, 3570, 3655, 3741, 3828,
       3916, 4005, 4095, 4186, 4278, 4371, 4465, 4560, 4656, 4753, 4851,
       4950], dtype=int32)
```
これをリストに変換します。

```
c = b.tolist()

// 一度に書くとこうなります。
// d = np.cumsum(np.array(df_list)).tolist() 

[結果]
[0, 1, 3, 6, 10, ...  4560, 4656, 4753, 4851, 4950]
```

最後に itertools。

```
import itertools

iter = itertools.accumulate(df_list)
iter_list = list(iter)
print(iter_list)

[結果]
[0, 1, 3, 6, 10, ...  4560, 4656, 4753, 4851, 4950]
```

##### 参考記事

[Python（Pandas）にて累積和（累積値）を計算する方法【cumsum関数】](https://life-freedom888.com/pandas-cumsum/)

[pandasで累積和・累積積（cumsum, cumprod, cummax, cummin）](https://note.nkmk.me/python-pandas-cumsum-cumprod/)

[Pythonで累積和・累積積（itertools.accumulate）](https://note.nkmk.me/python-itertools-accumulate/)

[【Python】pandasとnumpyの違いと速度比較](https://resanaplaza.com/2021/09/19/%E3%80%90python%E3%80%91pandas%E3%81%A8numpy%E3%81%AE%E9%81%95%E3%81%84%E3%81%A8%E9%80%9F%E5%BA%A6%E6%AF%94%E8%BC%83/)

[NumPyで累積和・累積積（np.cumsum, np.cumprod）](https://note.nkmk.me/python-numpy-cumsum-cumprod/)

[NumPy配列ndarrayとPython標準のリストを相互に変換](https://note.nkmk.me/python-numpy-list/)

[Python Pandas：リストをデータフレームに変換する方法](https://tech-branch.9999ch.com/archives/408)

[すごいぞitertoolsくん](https://qiita.com/anmint/items/37ca0ded5e1d360b51f3)

##### お薦め

[【最強のコロナ対策】のど飴の殺菌成分がコロナ変異株を99%以上不活性化、コロナ茶番完全終了](https://rapt-plusalpha.com/52886/)

[創価企業「カルチュア・コンビニエンス・クラブ（CCC)」がTカードの個人データ販売を本格化へ　中国による対日工作や詐欺などの犯罪に利用される恐れ](https://rapt-plusalpha.com/52872/)

[【振り込め詐欺の元締めは日本財団だった!!】 振り込め詐欺の犯罪グループによって集められ、被害者に返還されなかった“50億円”が日本財団に流れていた](https://rapt-plusalpha.com/52828/)

[【岸田首相と統一教会の切っても切れない関係】 勝共連合を設立した笹川良一と岸田家は親戚であり、どちらも中国人だった!!](https://rapt-plusalpha.com/52747/)

[【伊豆箱根バス】ノーマスクの女性客を強制的に下車させ、道路運送法違反、車両使用停止処分に マスク未着用との理由で、乗車拒否することは法律違反](https://rapt-plusalpha.com/52842/)

{{<youtube BScDlaO_rVA>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/Mina%E3%83%A9%E3%82%B8%E3%82%AA2/06bafba746f29b1106b31819bc48cba1e41690f1?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
