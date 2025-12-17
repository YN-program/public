---
title: "【CSS】grid layout -その２-"
description: ""
date: "2023-02-01T11:14:35+09:00"
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

グリッドレイアウトの基本の続きです。

各要素の名前、グリッド線名によるアイテムの配置、および`minmax()`などについて。

<!--more-->

#### {{<spanred>}}〇 使用例{{</spanred>}}
{{<file>}}

  <style>
      @charset "utf-8"

      @supports (grid-area: auto) {
          body {
              display: grid;
          }
      } 
      li {list-style:none;}
      *, *::before, *::after{
          box-sizing: border-box;
      }
      main {
          margin:50px;
          place-items: center;
      }

      .grid_container1{
          display: grid; /* 又は inline-glid */
          grid-template-columns:repeat(3, 1fr);
          grid-template-rows:repeat(2, 1fr);
          gap: 20px;
          background:#fddbff;
          margin: 20px;
          padding:20px;
          text-align:center;
          position: relative;
      }
      .grid_container1 div {
          font-weight:bold;
          background:#fff;
      }
      .grid_container2{
          display: grid;
          grid-template-columns: [col1] 1fr [col2] 1fr [col3] 1fr [col4];
          grid-template-rows: [row1] 50px [row2] 70px [row3] 100px [row4];
          gap: 20px;
          background:#fddbff;
          margin: 20px;
          padding:20px;
          text-align:center;
          position: relative;
      }
      .grid_container2 li {
          font-weight:bold;
          background:#fff;
      }
      .item1 {
        grid-column:col1 / col4;
        grid-row:row1 / row2;
        /* 
        grid-column-start: col1;
        grid-column-end: col4;
        grid-row-start: row1;
        grid-row-end: row2;
        */

      }
      .item2 {
        grid-column:col1 / col3;
        grid-row:row2 / row3;
        /* 
        grid-column-start: col1;
        grid-column-end: col3;
        grid-row-start: row2;
        grid-row-end: row3;
        */
      }
      .item3 {
        grid-column:col3;
        grid-row:row2;        
        /*
        grid-column-start: col3;
        grid-row-start: row2;
        */
      }
      .item4 {
        grid-column:col1 / col4;
        grid-row:row3;        
        /*
        grid-column-start: col1;
        grid-column-end: col4;
        grid-row-start: row3;
        */
      }

      /* @media screen and (max-width:900px){
          .grid_container{
              grid-template-rows:repeat(4, 100px);
              grid-template-columns:1fr;
              grid-template-areas:"a a a" "a a a" "b b b" "c c c";
          }   */

      }
  </style>

  <main>
      <div class="grid_container1">            
          <div>
              <p>アイテム１</p>
          </div>        
          <div>
              <p>アイテム２</p>
          </div>
          <div>
              <p>アイテム３</p>
          </div>  
          <div>
              <p>アイテム４</p>
          </div>
          <div>
              <p>アイテム５</p>
          </div>
          <div>
              <p>アイテム６</p>
          </div>        
      </div>
      <p>〇コンテナ：グリッド全体を囲む要素。</p>
      <p>〇アイテム：コンテナ内の直接の子要素（孫要素はアイテムではない）。</p>
      <p>〇ライン：グリッドを分ける線。左端あるいは上から1,2,3,4...と番号が振られる。また、右端あるいは下から-1,-2...と負の番号が振られる。</p>
      <p>〇トラック：grid-template-columns および grid-template-rows で定義された行と列において、任意の 2 本の線の間にある空間。</p>
      <p>〇セル：4つの交差するグリッド線に囲まれた領域のこと。cssグリッドの最小単位。</p>
      <p>〇エリア：一つ以上のセルからなる長方形の領域のこと。T字型やL字型などの領域を作ることはできない。</p>

      <hr>
      <p> グリッド線には名前を付けることができる（全ての線に名前を付ける必要はない）。<br>
          * grid-template-columns: [col1] 1fr [col2] 1fr [col3] 1fr [col4]; <br>
          * grid-template-rows: [row1] 50px [row2] 70px [row3] 100px [row4];
      </p>
      <p>これにより、名前でアイテムを配置することができる。</p>

      <ul class="grid_container2">
          <li class="item1">col1~col4, row1~row2</li>
          <li class="item2">col1~col3, row2~row3</li>
          <li class="item3">col3~col4, row2~row3</li>
          <li class="item4">col1~col4, row3~row4</li>
      </ul>

  </main>
{{</file>}}

上記レイアウトのコード。

`html`
```
<ul class="grid_container">
    <li class="item1">col1~col4, row1~row2</li>
    <li class="item2">col1~col3, row2~row3</li>
    <li class="item3">col3~col4, row2~row3</li>
    <li class="item4">col1~col4, row3~row4</li>
</ul>
```
`css`
```
.grid_container{
    display: grid;
    grid-template-columns: [col1] 1fr [col2] 1fr [col3] 1fr [col4];
    grid-template-rows: [row1] 50px [row2] 70px [row3] 100px [row4];
    gap: 20px;
    background:#fddbff;
    margin: 20px;
    padding:20px;
    text-align:center;
    position: relative;
}
.grid_container li {
    font-weight:bold;
    overflow:hidden;
    background:#fff;
}
.item1 {
  grid-column:col1 / col4;
  grid-row:row1 / row2;
  /* 
  grid-column-start: col1;
  grid-column-end: col4;
  grid-row-start: row1;
  grid-row-end: row2;
  */

}
.item2 {
  grid-column:col1 / col3;
  grid-row:row2 / row3;
  /* 
  grid-column-start: col1;
  grid-column-end: col3;
  grid-row-start: row2;
  grid-row-end: row3;
  */
}
.item3 {
  grid-column:col3;
  grid-row:row2;        
  /*
  grid-column-start: col3;
  grid-row-start: row2;
  */
}
.item4 {
  grid-column:col1 / col4;
  grid-row:row3;        
  /*
  grid-column-start: col1;
  grid-column-end: col4;
  grid-row-start: row3;
  */
}
```
***

`grid-template-columns`だけを指定すると、行は暗黙的に作られる。

`grid-auto-rows`で、暗黙的に作成された行のサイズを指定できる。（`grid-auto-columns`では、列のサイズを指定できる）

以下は、`grid-template-columns: repeat(3, 1fr);`、`grid-auto-rows: 50px;`
{{<file>}}

<style>
  .grid_container3{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: 50px;
    background:#fddbff;
    gap: 10px;
    margin: 20px;
    padding:20px;
    text-align:center;
    position: relative;
    counter-reset: number;
  }
  .grid_container3 li{
    background:#fff;

  }
  .grid_container3 li::before{
      counter-increment: number;
      content:counter(number);
    } 

</style>

  <ul class="grid_container3">
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
  </ul>

{{</file>}}

***

`minmax()`

トラックのサイズ指定において、最小のサイズと内容物に合わせたサイズを同時に指定できる。

以下は、`grid-template-columns: repeat(3, 1fr);`、`grid-auto-rows: minmax(20px, auto);`

{{<file>}}

<style>
  .grid_container4{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: minmax(20px, auto);
    background:#fddbff;
    gap: 10px;
    margin: 20px;
    padding:20px;
    text-align:center;
    position: relative;
    counter-reset: number;
  }
  .grid_container4 li{
    background:#fff;

  }
  .grid_container4 li::before{
      counter-increment: number;
      content:counter(number);
    } 

</style>

  <ul class="grid_container4">
    <li></li>
    <li>：内容物に合わせて、高さが変化する。</li>
    <li></li>
    <li></li>
    <li></li>
  </ul>

{{</file>}}

##### 参考記事

https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout#the_grid_container

https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Grid_Layout/Layout_using_Named_Grid_Lines

##### お薦め

[この世が良心的な人の生活しやすい世界になってほしい。それが私の願いです。](https://rapt-neo.com/?p=12236)

[新しいアイデアと発想を生み出すために必要なたった一つのこと。](https://rapt-neo.com/?p=9163)

[悪魔崇拝者を根本的に滅ぼし尽くす方法。それは「霊界」の奥義を知ることから始まります。](https://rapt-neo.com/?p=19013)

[天皇がどこからどう見ても悪魔崇拝者であるという証拠。](https://rapt-neo.com/?p=16687)

{{<youtube 4zYsDE5XH6s>}}

{{<tweet user="Rapt_plusalpha" id="1618891557566038016">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1621116511195926528">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1621093652922769408">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1621089024122454018">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1621084561643040769">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1620753941402365953">}}
&nbsp;
