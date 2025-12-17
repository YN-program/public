---
title: "【Pandas】ある一定の期間のデータを取り出して、欠損する日付を埋めて、存在しないデータを0で埋める"
description: ""
date: "2022-08-16T16:22:21+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Python"
  - "pandas"
menu:
main:
  name: プログラミング
  weight: 2
---
先日、python の defaultdict を用いて辞書の重複した値のキーの数を取得しました。

[【Python】辞書の重複した値のキーの数を取得する](https://progress-advance.com/programming/python-defaultdict/)

今回は pandas、django-pandas で、データをDataFrame形式にし、重複するデータの数を出して、飛び飛びになっている時系列データの欠損を埋めてみました。

<!--more-->

モデルの例

```
class Test(models.Model):

    class Meta:
        db_table    = "test"

    id   = models.UUIDField( default=uuid.uuid4, primary_key=True, editable=False )
    dt   = models.DateTimeField(verbose_name="",default=timezone.now)
    test = models.CharField(verbose_name="", max_length=50)
```

##### データの整形方法

`views.py`

```
from django_pandas.io import read_frame
import pandas as pd
from datetime import date, timedelta

test = Test.objects.all()

df = read_frame(test, fieldnames=['dt', 'test'])
df["date"] = [ d.date() for d in df.dt ]
a = df.loc[:,['date','test']]

b = a.pivot_table(index = ['date'], aggfunc = 'size')
c = b.reset_index()
d = c.set_axis(['date','test_num'],axis=1)

## 一度に書くとこうなる 
##　e = a.pivot_table(index = ['date'], aggfunc = 'size').reset_index().set_axis(['date','test_num'],axis=1)

print("====================")
print(b)
print("====================")
print(c)
print("====================")
print(d)
print("====================")

## (結果)
====================
date
2022-08-12    4
dtype: int64
====================
         date  0
0  2022-08-12  4
====================
         date  test_num
0  2022-08-12         4
====================

```

次に、オブジェクトをある一定の期間で取り出して、欠損する日付を埋めて、存在しないデータを0で埋める場合。

```
today = timezone.now()
past_day = today - datetime.timedelta(days=28)

test = Test.objects.filter(dt__gte=past_day, dt__lte=today).order_by("dt")

df = read_frame(test, fieldnames=['dt', 'test'])
df["date"] = [ d.date() for d in df.dt ]
a = df.loc[:,['date','test']]

b          = a.pivot_table(index = ['date'], aggfunc = 'size').reset_index().set_axis(['date','test_num'],axis=1)
start_time = pd.to_datetime(past_day.date())
end_time   = pd.to_datetime(today.date())
time_list  = pd.date_range(start_time, end_time, freq='D')
df         = b.set_index('date').reindex(time_list, fill_value=0)


## (結果)

        　　　   test_num
2022-07-19             0
2022-07-20             0
2022-07-21             0
2022-07-22             0
...

2022-08-12             4
2022-08-13             0
2022-08-14             0
2022-08-15             0
```


##### 参考記事

[django-pandas 0.6.6](https://pypi.org/project/django-pandas/)

[DjangoでPandas（DataFrame)を活用する方法](https://sinyblog.com/django/django-pandas/)

[Pandasの日付が縦に並んでいるデータフレームで、欠けている日付の行を補う方法と気をつけること](https://qiita.com/hanon/items/29cf5ed9acb4f731538f)

[Pandas.DataFrameでとびとびの時系列データを補完する](https://qiita.com/aras/items/17551669c3b0ba8f60bd)

[[python] pandasのreindexとdate_rangeを利用して、時系列データの欠損を埋める](https://wayama.io/article/library/python/022/)

[pandas.DataFrameの行名・列名の変更](https://note.nkmk.me/python-pandas-dataframe-rename/)

##### お薦め

[【静岡県浜松市】コロナ陽性者5万人を調査した結果、ワクチンを何度接種しても重症化率に変わりがないことが判明](https://rapt-plusalpha.com/51331/)

[【中国・恒大集団】電気自動車（EV）事業に参入するも、生産工場が空の状態になっていることが判明　資金繰り悪化で生産困難か](https://rapt-plusalpha.com/51309/)

[自分の中から聖書に書かれた罪をなくすことで、不安や心配がなくなり、喜びの涙で溢れる毎日を生きられるようになった（十二弟子・ミナさんの証）](https://rapt-plusalpha.com/51136/)

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/Mina%E3%83%A9%E3%82%B8%E3%82%AA2/06bafba746f29b1106b31819bc48cba1e41690f1?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/03/8f93e98f37bf3eb81aafe72ad2e7f0423a4560a6?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}