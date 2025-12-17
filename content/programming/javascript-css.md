---
title: "【Javascript】class操作のまとめ"
description: ""
date: "2022-11-25T08:23:37+09:00"
thumbnail:
  src: "img/thumb/programming.jpg"
  visibility:
    - list
categories:
  - "programming"
tags:
  - "Javascript"
menu:
main:
  name: プログラミング
  weight: 2
---
Javascriptでclassの追加や削除、置換などを行う方法のメモです。

<!--more-->

〇classの追加

```
const target = document.ElementById('id');

target.classList.add('classA');

//複数追加する場合
target.classList.add('classA','classB','classC');

//別の方法(この場合、classDの前にスペースが必要)
target.className += ' classD';

```

〇classの削除

```
const target = document.ElementById('id');

target.classList.remove('classA');

//複数削除する場合
target.classList.remove('classA','classB','classC');
```

〇classの切り替え
```
const target = document.ElementById('id');

target.classList.toggle('classA');
//classAがあれば削除、なければ追加になる。
```

〇classの置換
```
const target = document.ElementById('id');

//classAをclassBに置き換える。
target.classList.replace('classA','classB');

//ある要素のクラスを完全に置き換える。元々 classA, classBであった場合、classC, classDに置き換わる。
target.className = "classC classD";

```

〇classの存在確認

```
const target = document.ElementById('id');

target.classList.contains('classA');

```

〇classの数を取得

```
const target = document.ElementById('id');

target.classList.length;
```

〇class属性に指定されている文字列の取得

```
const target = document.ElementById('id');

target.classList.value;
```

〇class名の取得

```
const target = document.ElementById('id');

target.className;
```

〇例

```
if (target.classList.contains('classA') ){
    target.classList.replace('classA','classB');
} else {
    target.classList.replace('classB','classA');
}
```

##### 参考記事

[【JavaScript】クラス操作まとめ（追加、削除、トグル、置換など）](https://eclair.blog/javascript-classlist-methods/#JavaScript-6)

[JavaScript className](https://www.javascripttutorial.net/javascript-dom/javascript-classname/)

[HTML DOM Element className](https://www.w3schools.com/jsref/prop_html_classname.asp)

##### お薦め

[悪魔を拝んで人生を破壊する生き方から、神様を拝んで人生を幸福にする生き方へ。](https://rapt-neo.com/?p=26065)

[悪魔に打ち勝ち、自分の心身を守る方法。祈って聖霊を受けることが、悪魔を滅ぼす最大の鍵です。](https://rapt-neo.com/?p=25914)

[一人一人が悪魔の誘惑に打ち勝つことが、この世界を変えることになります。先ずは自分のためにお祈りしてみてください。](https://rapt-neo.com/?p=25934)

[何をどう祈ればいいのか、具体的な方法をお教えします。自分の人生も世界の運命も変える鍵がここにあります。](https://rapt-neo.com/?p=25977)

[悪魔が一夜にして滅びることを望むよりも先に、あなた自身がこの世の改革者になることを私は望みます。](https://rapt-neo.com/?p=26034)

{{<tweet user="Rapt_plusalpha" id="1595380387458347008">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1595368117881929730">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1595775124393062400">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1595746829668585472">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1595732057702993923">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1595723087051300864">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1595717085883404290">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1595415593259511809">}}
&nbsp;