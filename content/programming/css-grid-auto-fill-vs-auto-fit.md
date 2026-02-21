---
title: "【CSS】grid layout -その３- auto-fillとauto-fit"
description: ""
date: "2023-02-05T23:36:02+09:00"
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
グリッドレイアウトのその３。

`repeat()`と`minmax()`、`auito-fit`、`auto-fill`の使用例。

<!--more-->

`grid-template-columns:repeat(auto-fill, minmax(50px, 1fr));`とすると、幅50px~1frのセルが作られる。グリッドコンテナの幅が拡大するにつれ、余白に新たな空のセルが追加される。コンテナ幅が小さくなると、アイテムは自動的に折り返される。

`grid-template-columns:repeat(auto-fit, minmax(50px, 1fr));`では、グリッドコンテナの幅に合わせてアイテム幅も拡大する。折り返されたときの空きスペースは、余白になる（セルは追加されない）。


![auto-fill-auto-fit](/img/css/a.png)

![auto-fill-auto-fit](/img/css/b.png)

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
          margin:20px;
          place-items: center;
      }
      .content ul {margin:0;}
      .grid_container1{
          display: grid;
          grid-template-columns:repeat(auto-fill, minmax(50px, 1fr));
          background:#f0f0f0;
          margin: 20px;
          padding:20px;
          text-align:center;
          position: relative;
          counter-reset: number;
      }
      .grid_container1 li {
          font-weight:bold;
          background:#fff;
          border:1px solid black;
      }
      .grid_container1 li::before{
        counter-increment: number;
        content:counter(number);
      } 
      .grid_container2{
          display: grid;
          grid-template-columns:repeat(auto-fit, minmax(50px, 1fr));
          background:#f0f0f0;
          margin: 20px;
          padding:20px;
          text-align:center;
          position: relative;
          counter-reset: number;
      }
      .grid_container2 li {
          font-weight:bold;
          background:#fff;
          border:1px solid black;
      }
      .grid_container2 li::before{
        counter-increment: number;
        content:counter(number);
      }
      .mark_area {padding:0 0.25rem;}      
  </style>

  <main>
    <mark class="mark_area"> grid-template-columns:repeat(auto-fill, minmax(50px, 1fr));</mark>
      <ul class="grid_container1">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
      <br>
    <mark class="mark_area"> grid-template-columns:repeat(auto-fit, minmax(50px, 1fr)); </mark>
      <ul class="grid_container2">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
  </main>
{{</file>}}

&nbsp;

簡単になりますが、以上です。

##### 参考記事

[Auto-Sizing Columns in CSS Grid: auto-fill vs auto-fit](https://css-tricks.com/auto-sizing-columns-css-grid-auto-fill-vs-auto-fit/)

[横幅が広がったときの挙動が変わる！CSS Gridの「auto-fill」と「auto-fit」の違い
](https://webrandum.net/css-grid-auto-fill-auto-fit/)

[【CSS】auto-fit・auto-fillの使い方、トラックの幅の指定を繰り返す!
](https://shu-naka-blog.com/css/auto-fit-fill/)

##### お薦め

{{<x user="Rapt_plusalpha" id="1622566802952982528">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1622565446670249984">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1622556016184524807">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1622552123769769986">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1622546290671489024">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1622544866193571840">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1622543396765331457">}}
&nbsp;