---
title: "リストからNoneを削除"
description: ""
date: "2022-10-01T15:31:47+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "django"
  - "python"
  - "pandas"
menu:
main:
  name: プログラミング
  weight: 2
---
python, pandas でリストから None を削除する方法メモ。

<!--more-->

```
import pandas as pd

df = pd.DataFrame(
    data={'01': pd.Series([None,'Tokyo','Chiba','Tokyo',None,'Chiba','Kanagawa','Chiba','Tokyo','Saitama',None]),
          }
)
df

結果：
          01
0       None
1      Tokyo
2      Chiba
3      Tokyo
4       None
5      Chiba
6   Kanagawa
7      Chiba
8      Tokyo
9    Saitama
10      None
```


```
df.dropna()

結果：

         01
1     Tokyo
2     Chiba
3     Tokyo
5     Chiba
6  Kanagawa
7     Chiba
8     Tokyo
9   Saitama

```

リスト内包表記を使用する場合。
```
test = [None,'Tokyo','Chiba','Tokyo',None,'Chiba','Kanagawa','Chiba','Tokyo','Saitama',None]

test1 = [t for t in test if t is not None]
test1

結果：
['Tokyo', 'Chiba', 'Tokyo', 'Chiba', 'Kanagawa', 'Chiba', 'Tokyo', 'Saitama']
```


重複を削除する場合。

```
test2 = set(test1)
test2

結果：
{'Chiba', 'Kanagawa', 'Saitama', 'Tokyo'}
```

##### 参考記事

[pandasで欠損値NaNを削除（除外）するdropna](https://note.nkmk.me/python-pandas-nan-dropna/)

[Python リスト内からNoneを除去する方法](https://teratail.com/questions/110578)

[[python]リストからNoneを除外する簡単な書き方](https://dackdive.hateblo.jp/entry/2014/08/25/172414)

[【Python】pandasのデータフレームを作成する方法6つ](https://www.self-study-blog.com/dokugaku/python-pandas-dataframe-make/)

##### お薦め

{{<youtube FXr2626vR58>}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1575846871674736640">}}

{{<x user="Rapt_plusalpha" id= "1575837298461052928">}}

{{<x user="Rapt_plusalpha" id= "1575820191019061248">}}

{{<x user="Rapt_plusalpha" id= "1575795141658308610">}}

{{<x user="Rapt_plusalpha" id= "1575449677847179266">}}

