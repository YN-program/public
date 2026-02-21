---
title: "【CSS】grid layout -その１-"
description: ""
date: "2023-01-30T23:33:32+09:00"
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

グリッドレイアウトが便利なので、基本的なことから学んだことを複数回に分けて記載していきたいと思います。

<!--more-->

#### {{<spanred>}}〇 grid-containerの記載例{{</spanred>}}


```
.grid-container {
    grid-template-columns:;
    /*
    1fr 1fr 1f
    minmax(100px, 1fr) 1fr
    repeat(3, 1fr)
    100px auto 100px 1fr
    */

    grid-template-rows:;
    /*
    min-content min-content 1fr
    max-content 100px 1fr    
    70% 30%
    50% 100px auto
    */

    grid-template-areas:;
    /*
    "header header header"
    "main main . sidebar"
    "footer footer footer";
    */

    gap: 20px 50px;
}
.item1 { grid-area: header;}
.item2 { grid-area: main;}
.item3 { grid-area: sidebar;}
.item4 { grid-area: footer;}

```

fr は外枠のサイズに合わせてグリッドを分割して、サイズを自動的に調節してくれる。

`1fr 1fr 1fr`は3分割になる。

auto は、同じ行列にfrがある場合は、グリッド内のコンテンツに合わせてサイズを調節し、frがない場合は、1frと同じ働きをする。

`grid-template-areas`は、grid-area で指定されたグリッド領域の名前を参照し、グリッドテンプレートを定義する。グリッドエリア名を繰り返すと、そのセルにまたがってコンテンツが表示さる。ピリオドは空のセルを表す。

`gap`は`row-gap`, `column-gap`の短縮形で、値は一つでも可。`gap:20px 50px;` は`row-gap:20px; column-gap:50px;`と同じ。

#### {{<spanred>}}〇 使用例{{</spanred>}}

{{<file>}}

  <style>
      @charset "utf-8"

      @supports (grid-area: auto) {
          body {
              display: grid;
          }
      } 

      *, *::before, *::after{
          box-sizing: border-box;
      }
      main {
          margin:50px;
          place-items: center;
      }

      .grid_container{
          display: grid; /* 又は inline-glid */
          grid-template-columns:1fr 1fr 1fr;
          grid-template-rows:repeat(3, 100px);
          grid-template-areas:"a a c" "a a c" "b b c";
          gap: 20px 50px;
          background:#fff;
          margin: 20px;
          padding:20px;
          text-align:center;
          position: relative;
          overflow:scroll;
      }
      .grid_container div {
          font-size:20px;
          font-weight:bold;
          overflow:hidden;
      }
      .grid_container .a {
          background:#fff4ff;
          grid-area:a;
      }
      .grid_container .b {
          grid-area:b;
          background:#f9f4ff;
      }
      .grid_container .c {
          grid-area:c;
          background:#f4f4ff;
      }
      @media screen and (max-width:900px){
          .grid_container{
              grid-template-rows:repeat(4, 100px);
              grid-template-columns:1fr;
              grid-template-areas:"a a a" "a a a" "b b b" "c c c";
          }  

      }
  </style>

  <main>
      <div class="grid_container">            
          <div class="a">
              <p>text1</p>
          </div>        
          <div class="b">
              <p>text2</p>
          </div>
          <div class="c">
              <p>text3</p>
          </div>
      </div>
  </main>
{{</file>}}

`html`

```
<main>
    <div class="grid_container">            
        <div class="a">
            <p>text1</p>
        </div>        
        <div class="b">
            <p>text2</p>
        </div>
        <div class="c">
            <p>text3</p>
        </div>
    </div>
</main>

```

`css`
```
main {
    margin:50px;
    place-items: center;
}

.grid_container{
    display: grid; /* 又は inline-glid */
    grid-template-columns:1fr 1fr 1fr;
    grid-template-rows:repeat(3, 100px);
    grid-template-areas:"a a c" "a a c" "b b c";
    gap: 20px 50px;
    background:#fff;
    margin: 20px;
    padding:20px;
    text-align:center;
    position: relative;
    overflow:scroll;
}
.grid_container div {
    font-size:20px;
    font-weight:bold;
    overflow:hidden;
}
.grid_container .a {
    background:#fff4ff;
    grid-area:a;
}
.grid_container .b {
    grid-area:b;
    background:#f9f4ff;
}
.grid_container .c {
    grid-area:c;
    background:#f4f4ff;
}
@media screen and (max-width:760px){
    .grid_container{
        grid-template-rows:repeat(4, 100px);
        grid-template-columns:1fr;
        grid-template-areas:"a a a" "a a a" "b b b" "c c c";
    }  
}     
```

{{<file>}}
<style>
    .sample{ 
        width:min-content;
        background:lightgray;
        margin:0.25rem;
    }
    .sample2{
        width:max-content;
        background:lightgray;
        margin:0.25rem;
    }
    .sample3{
        width:fit-content;
        background:lightgray;
        margin:0.25rem;
    }
</style>
<li>widthがmin-contentの場合、要素のコンテンツで最も長い単語の幅に合わせる。</li>
<div class="sample">テキスト</div>
<div class="sample">min-conten</div>
<div class="sample">sample text</div>
<hr>
<li>widthがmax-contentの場合、要素の幅はコンテンツの幅になる。コンテンツがオーバーフローしても、テキストは折り返されない。</li>
<div class="sample2">テキスト</div>
<div class="sample2">max-content</div>
<div class="sample2">sample text.sample text.sample text.sample text.sample text.sample text.</div>
<hr>
<li>widthがfit-contentの場合、要素の幅はコンテンツの幅になる。利用可能な領域を使い、max-contentを超えることはない。</li>
<li>width ・ height ・ min-width ・ min-height ・ max-width ・ max-height のレイアウトボックスサイズとして使う場合、 最大 ・ 最小サイズはコンテンツのサイズに対応する。</li>
<div class="sample3">テキスト</div>
<div class="sample3">fit-content</div>
<div class="sample3">sample text. sample text. sample text. sample text. sample text. sample text. </div>

{{</file>}}

&nbsp;

##### 参考記事

https://kumonosublog.com/coding/css/grid_layout_fr_auto/

[A Complete Guide to CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/#top-of-site)

https://www.w3schools.com/css/css_grid.asp

https://developer.mozilla.org/en-US/docs/Web/CSS/min-content

https://developer.mozilla.org/en-US/docs/Web/CSS/max-content

https://developer.mozilla.org/en-US/docs/Web/CSS/fit-content

##### お薦め

{{<x user="Rapt_plusalpha" id="1620053311394287617">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1620036390242381825">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1620024546727456768">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1620007146736992257">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1619683930025918464">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1619654744896856065">}}
&nbsp;