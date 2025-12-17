---
title: "【Python】月初め、月末の取得方法　ー訂正ー"
description: ""
date: "2022-09-13T16:51:45+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "django"
  - "python"
menu:
main:
  name: プログラミング
  weight: 2
---
月間データ分析をするための、月初めと月末の取得方法。

<!--more-->

datetime型の場合

```
import datetime
import calendar

today = datetime.date.today()
first_day=today.replace(day=1)
eom = first_day.replace(day=calendar.monthrange(first_day.year, first_day.month)[1])
print(first_day,eom)

## 結果 ##
2022-09-01 2022-09-30

```

`timezone.now()`を使う場合。

```
from django.utils import timezone

today = timezone.now()
t = today.replace(day=1, hour=0, minute=0, second=0)
first_day = datetime.datetime(t.year, t.month, t.day, 0, 0, 0, 0,
                        tzinfo=datetime.timezone(datetime.timedelta(hours=9)))  #月始め
eom = first_day.replace(day=calendar.monthrange(first_day.year, first_day.month)[1])
print(first_day,eom)

## 結果 ##
2022-09-01 00:00:00+09:00 2022-09-30 00:00:00+09:00

```
一応上手くいくが、月末の時間が 23:59:59+09:00 になってほしいので、更に追加。（この部分を訂正しました（文末））

```
today = timezone.now()
t = today.replace(day=1, hour=0, minute=0, second=0)
first_day = datetime.datetime(t.year, t.month, t.day, 0, 0, 0, 0,
                            tzinfo=datetime.timezone(datetime.timedelta(hours=9))) #月始め
eom = first_day.replace(day=calendar.monthrange(first_day.year, first_day.month)[1])

eom1 = eom + datetime.timedelta(days=1)   ## 追加
eom2 = eom1 - datetime.timedelta(seconds=1) ## 追加

## 結果 ##
# first_day : 2022-09-01 00:00:00+09:00 
# eom : 2022-09-30 00:00:00+09:00
# eom1 : 2022-10-01 00:00:00+09:00
# eom2 : 2022-09-30 23:59:59+09:00

## 一行で書くとこうなります。
## eom = first_day.replace(day=calendar.monthrange(first_day.year, first_day.month)[1])  + datetime.timedelta(days=1)  - datetime.timedelta(seconds=1)

```

追記：2022/09/13 22:10

更に別の方法が見つかったので、追記します。

月単位で絞り込みをする場合、以下のようなシンプルなものでもOK。月初め、月末を求める必要がないので楽です。

```
Sample.objects.filter(date__year = '2022', 
                      date__month = '09')
```

先に書いた方法で、月初め、月末を求めて絞り込みをする場合は、以下のように書くことができます。

```
Sample.objects.filter(date__range = [first_day, eom])

```
```
Sample.objects.filter(date__gte = first_day, date__lte = eom ])
```


訂正記事

```
today = timezone.now()

t = today.replace(day=1, hour=0, minute=0, second=0)
this_month = datetime.datetime(t.year, t.month, t.day, 0, 0, 0, 0,
                              tzinfo=datetime.timezone(datetime.timedelta(hours=9)))  # 今月1日のタイムゾーン付き

last_month = this_month.replace(month=t.month - 1)　# 先月1日

## this_month 含まない絞り込み。
sample = Sample.objects.filter(dt__gte=last_month, dt__lt=this_month) 

## this_month を含む絞り込み。
sample = Sample.objects.filter(dt__range=[last_month, this_month])

## または、
sample = Sample.objects.filter(dt__gte=last_month, dt__lte=this_month) 

```
この方法では、最初の t の day を、例えば10に設定すると、先月の10日から今月の10日の間で絞り込むことが出来ます。

ただし、1月の場合、1月から1を引くことはできないので、if文で切り分けます。

```
if t.month !=1:
    last_month = this_month.replace(month=t.month - 1)
else:
    last_month = this_month.replace(year=t.year - 1, month=12)
```

度々の訂正、失礼しました。

##### 参考記事

[【Python】今月の集計をしたい時に使う月初の取得方法](https://qol-kk.com/wp2/blog/2020/12/11/post-2327/)

[Python, datetime, pytzでタイムゾーンを設定・取得・変換・削除](https://note.nkmk.me/python-datetime-pytz-timezone/)

[How do I filter query objects by date range in Django?](https://stackoverflow.com/questions/4668619/how-do-i-filter-query-objects-by-date-range-in-django)

##### お薦め

{{<youtube 0HQT34jbkls>}}
&nbsp;

{{<youtube ynhVt5evdDc>}}
&nbsp;

{{<youtube BScDlaO_rVA>}}
&nbsp;

{{<youtube mJSBdQ0oBPg>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/7/b54bf0d4d8050d14751fbb184a7b3357745a7169?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/8/2b598c7845f10b8f0a550aa65a60f47f973b80b3?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}