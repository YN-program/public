---
title: "【Python】辞書の重複した値のキーの数を取得する"
description: ""
date: "2022-08-13T10:34:44+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Python"
  - "defaultdict"
menu:
main:
  name: プログラミング
  weight: 2
---
例えば、次のようなモデルがあったときに、毎日いくつのデータが作られたのか知りたい。

そんな時に、`defaultdict`が便利だった。

<!--more-->

```
class Test(models.Model):

    class Meta:
        db_table    = "test"

    id   = models.UUIDField( default=uuid.uuid4, primary_key=True, editable=False )
    dt   = models.DateTimeField(verbose_name="",default=timezone.now)
    test = models.CharField(verbose_name="", max_length=50)
```

```
from collections import defaultdict

test = Test.objects.all()

dic = { t:t.dt.date() for t in test }

d = defaultdict(list)
for k, v in dic.items():
    d[v].append(k)

//リストの場合
result_list = [ (k, len(v)) for k,v in d.items()]
print(result_list)

//結果 [(datetime.date(2022, 8, 13), 1), (datetime.date(2022, 8, 7), 4), (datetime.date(2022, 8, 6), 3)]

//辞書の場合
result_dict = { k:len(v) for k, v in d.items() }
print(result_dict)

//結果 {datetime.date(2022, 8, 13): 1, datetime.date(2022, 8, 7): 4, datetime.date(2022, 8, 6): 3}

```

##### 参考記事

[python 辞書の重複した値のキーを取り出す](https://teratail.com/questions/196794)

[Python defaultdict の使い方](https://qiita.com/xza/items/72a1b07fcf64d1f4bdb7)

##### お薦め動画

{{<youtube -6BMQT_wmSc>}}
&nbsp;
{{<youtube gNaZBg3jl9g>}}
&nbsp;
{{<youtube SHDd4o7Kb2w>}}
&nbsp;
{{<youtube zvac-dxonQM>}}
&nbsp;
{{<youtube beqymEAlsKU>}}
&nbsp;
{{<youtube 4zYsDE5XH6s>}}
&nbsp;