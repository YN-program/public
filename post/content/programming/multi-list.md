---
title: "【Django】テンプレートで多重リストから要素を取り出す方法"
description: ""
date: "2022-10-27T11:18:23+09:00"
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
リスト、多重リストから要素を取り出すには、リスト名の後にインデックス番号を記述します。

<!--more-->

以下のようなリストがあった場合、

```
number = 100
sample_list = [["a","b","c","d"],
               ["e","f","g","h"],
               ["i","j","k","l"],
               number,
              None,
              []]
```

sample_list[0]と記載すると、['a', 'b', 'c', 'd']を、sample_list[2]と記載すると、['i', 'j', 'k', 'l']を取得することができます。

また、sample_list[0][0]と記載すると、'a'を取得することができます。

sample_list[3]と記載するとnumberの100を取得することができます。

sample_list[4]はNoneなので何も返ってきませんが、sample_list[5]では空のリスト[]が返ってきます。

Djangoのviews.pyにてsample_listを作り、テンプレートで表示する場合は次のようにします。

```
{{ sample_list.0 }}  ## ['a', 'b', 'c', 'd']

{{ sample_list.0.0 }}  ## 'a'

{{ sample_list.0.0 }}  ## 'a'

{{ sample_list.3 }}  ## 100
```

views.pyにて複数のクエリセットや変数をリストに入れてテンプレートで取り出したい場合、次のようにすると要素を取得することができます。

`views.py`
```
qs_list = [qs0, qs1, number]

```

qs0, qs1 のフィールドに`title,contents,date`があった場合、

`template`

```
##qs0

{% for qs0 in qs_list.0 %}

  {{ qs0.title }} 

  {{ qs0.contents }}

  {{ qs0.date }}

{% endfor %}

##qs1

{% for qs1 in qs_list.1 %}

  {{ qs1.title }}

  {{ qs1.contents }}

  {{ qs1.date }}

{% endfor %}

## number

{{ qs_list.2 }}
```

あまり使う機会はないかもしれませんが、views.pyのリストが以下のように多重リストの場合。

```
qs_lists =　[[qs0, qs1, number], 
            [qs0, qs1, number], 
            [qs0, qs1, number], 
            [qs0, qs1, number], 
            ...]
```

`template`

```
{% for qs_list in qs_lists %}

    {% for qs0 in qs_list.0 %}

        {{ qs0.title }} 

        {{ qs0.contents }}

        {{ qs0.date }}

    {% endfor %}

    {% for qs1 in qs_list.1 %}

        {{ qs1.title }}

        {{ qs1.contents }}

        {{ qs1.date }}

    {% endfor %}

    {{ qs_list.2 }}　## number

{% endfor %}
```

以上になります。


##### 参考記事

[Pythonのリストからの要素の取り出し（抽出）方法のまとめ](https://www.headboost.jp/python-list-how-to-grab-a-value/)

[[Django]テンプレートでobjectsの配列番号を指定する方法](https://sleepless-se.net/2018/07/09/django-template/)

##### お薦め

{{<tweet user="Rapt_plusalpha" id="1584862349399556098">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1585247741890166784">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1585239610464284672">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1585236564216381440">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1585228213575262208">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1585220368855633920">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1585217783667003393">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1584505105256427522">}}
&nbsp;