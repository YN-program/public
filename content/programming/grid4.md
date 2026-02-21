---
title: "【CSS】grid layout -その４- justify-self, align-selfなど"
description: ""
date: "2023-02-16T10:59:45+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "CSS"
menu:
main:
  name: プログラミング
  weight: 2
---

`margin`や`justify-self`, `align-self`, `place-self`, `justify-items`, `align-items`, `place-items`などを使用して、グリッドアイテムを配置。


<!--more-->

![grid](/img/css/grid.png)

* item1: margin-left:auto;
* item2: margin-right:auto;
* item3: justify-self:start;
* item4: justify-self:end;
* item5: justify-self:center;
* item6: justify-self:stretch;
* item7: align-self:start;
* item8: align-self:end;
* item9: align-self:stretch;
* item10: align-self:stretch;
* item11: place-self:center;
* item12: place-self:start end;


`justify-self`

グリッドアイテムを行に沿って整列させる。一つ一つのアイテムに適用される。
* start：セルの開始位置にアイテムを設置。
* end：セルの終端にアイテムを設置。
* center:セルの中心にアイテムを設置。
* stretch：セルの幅に合わせてアイテムを設置（デフォルト）。

`align-self`

グリッドアイテムを列に沿って整列させる。一つ一つのアイテムに適用される。
* start：セルの開始位置にアイテムを設置。
* end：セルの終端にアイテムを設置。
* center:セルの中心にアイテムを設置。
* stretch：セルの高さに合わせてアイテムを設置（デフォルト）。

`place-self`

align-selfとjustify-selfの両方のプロパティを一度に設定できる。一つ一つのアイテムに適用される。

```
// place-selfの例
 
item1 {
    place-self:start center;
    ## 最初に align-self、次に justify-self。
}

item2 {
    place-self:center;
    ## 値が一つの場合、align-selfとjustify-selfの両方がこの値になる。
}
```

***
グリッドコンテナに対して、`justify-items`, `align-items`、`place-items`。

`justify-items`は、グリッドアイテムを行に沿って整列させる。コンテナ内の全てのアイテムに適用される。

 `align-items`は、グリッドアイテムを列に沿って整列させる。コンテナ内の全てのアイテムに適用される。

`place-items`は、`justify-items`と `align-items`の両方のプロパティを単一の宣言で設定できる。

最初の値は`align-items`、2番目の値は`justify-items`を設定する。2番目の値が省略された場合、最初の値が両方のプロパティに割り当てられる。


{{<file>}}

<style>


.grid_wrapper{
    display:grid;
    grid-template-columns:repeat(auto-fill, minmax(200px, 1fr));
    place-items: center;
    color:black;
}
.grid_wrapper div{
  margin-bottom:1rem;
}
.grid_wrapper li {
    list-style: none;
    font-size:1rem;
    background:#fff;
    border:solid 1px black;
    margin:0;
}
.grid_wrapper ul { margin:0; }

mark { margin:0.25rem;}
.grid_container{
    display: grid;
    grid-template:100px 100px/100px 100px;
    background:#f7f7f7;
    text-align:center;
    position: relative;
}
.grid_container1{
    display: grid;
    width:200px;
    grid-template-columns:repeat(2,100px);
    background:#f7f7f7;
    text-align:center;
    position: relative;
}
.item_width {width:70px;}

.a { justify-items:start; }
.b { justify-items:end; }
.c { justify-items:center; }
.d { justify-items:stretch; }
.e { align-items:start; }
.f { align-items:end; }
.g { align-items:center; }
.h { align-items:stretch; }
.i { place-items:start; }
.j { place-items:end; }
.k { place-items:center; }
.l { place-items:stretch; }
.m { place-items:start end; }
.n { place-items:end center; }
.o { place-items:center start; }
.p { place-items:center end; }

</style>

<div class="grid_wrapper">

  <div>
    <mark>justify-items: start;</mark>
    <ul class="grid_container a">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>justify-items: end;</mark>
    <ul class="grid_container b">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>justify-items: center;</mark>
    <ul class="grid_container c">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>justify-items: stretch;</mark>
    <ul class="grid_container d">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>align-items: start;</mark>
    <ul class="grid_container e">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>align-items: end;</mark>
    <ul class="grid_container f">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>align-items: center;</mark>
    <ul class="grid_container g">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>align-items: stretch;</mark>
    <ul class="grid_container h">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>

  
  <div>
    <mark>place-items: start;</mark>
    <ul class="grid_container i">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>place-items: end;</mark>
    <ul class="grid_container j">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>place-items: center;</mark>
    <ul class="grid_container k">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>place-items: stretch;</mark>
    <ul class="grid_container l">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>place-items: start end;</mark>
    <ul class="grid_container m">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>place-items: end center;</mark>
    <ul class="grid_container n">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>place-items: center start;</mark>
    <ul class="grid_container o">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>
  <div>
    <mark>place-items: center end;</mark>
    <ul class="grid_container p">
        <li>item1</li>
        <li>item2</li>
        <li>item3</li>
        <li>item4</li>
    </ul>
  </div>

</div>
{{</file>}}



##### お薦め

{{<x user="Rapt_plusalpha" id="1626190911150653441">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1626181433479041026">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1626173988811702275">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1626171014941130754">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1626158455638884353">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1625800365701742592">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1625797841309544449">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1625443477696966656">}}
&nbsp;