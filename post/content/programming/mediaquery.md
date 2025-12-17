---
title: "【CSS】メディアクエリ orientation,hover, etc."
description: ""
date: "2023-01-10T09:52:59+09:00"
thumbnail: 
  src: "img/thumb/html.jpg"
  visibility:
  - list
categories:
  - "programming"
tags:
  - "CSS"
menu:
main:
  name: プログラミング
  weight: 2
---
メディアクエリは、画面サイズや画面解像度などの閲覧状況に応じて、cssを切り替えることができる。便利なところをメモ。

<!--more-->

`@media`で指定した条件のときに、cssを適用することができる。

〇使用例

```
/* 画面サイズ（横幅）が768px以上の場合に適用される */
@media screen and (min-width: 768px) {
   .sample { width: 100px; }
}

/* 画面サイズ（横幅）が769～1024pxの場合に適用される */
@media screen and (min-width: 769px) and (max-width: 1024px){
   .sample { width: 200px; }
}

@media screen and (orientation: landscape) {
   .sample { background: gray; }
}

@media screen and (orientation: portrait) {
   .sample { background: lightgray; }
}

```

***

`orientation`に指定できる値は、`landscape`と`portrait`。

`portrait`:デバイスのviewportの高さが横幅よりも大きいか等しい。

`landscape`:デバイスのviewportの横幅が高さよりも大きい。

{{<file>}}
<style>
  .sample{
    background:lightgray;
    width:100px;
    height:100px;
  }
  @media screen and (orientation: landscape) {
    .sample{
      background:orangered;
    }
  }
</style>

<div class=sample>画面幅が高さよりも大きいと赤色になる</div>

{{</file>}}

```
<style>
  .sample{
    background:lightgray;
    width:100px;
    height:100px;
  }
  @media screen and (orientation: landscape) {
    .sample{
      background:orangered;
    }
  }
</style>

<div class=sample></div>
```
***

{{<file>}}
<style>
  .sample2{
    background:lightgray;
    width:300px;
    height:50px;
  }

  /*
  css level4　（最新のブラウザではサポートされている） では、@media (min-width: 768px) and (max-width: 1024px) の代わりに
  以下のように記載できる。768～1024pxの間だけ、赤色になる。
  */

  @media (768px <= width <= 1024px) {
    .sample2{
      background:orangered;
    }
  }
</style>

<div class=sample2>
 @media (768px <= width <= 1024px)
 <br>768～1024pxの間だけ、赤色になる。
</div>
{{</file>}}

```
<style>
  .sample2{
    background:lightgray;
    width:300px;
    height:50px;
  }

  /*
  css level4　（最新のブラウザではサポートされている） では、
  @media (min-width: 768px) and (max-width: 1024px) の代わりに
  以下のように記載できる。768～1024pxの間だけ、赤色になる。
  */

  @media (768px <= width <= 1024px) {
    .sample2{
      background:orangered;
    }
  }
</style>

<div class=sample2></div>
```

***

〇ホバーができるデバイスだけホバーをさせる `@madia(hover:hover)`

{{<file>}}

<style>
.sample3 {
  background: lightgray;
  color: black;
}
.sample3:hover {
  background: yellow;
}
@media (hover: hover) {
  .sample3:hover {
    color: white;
    background: black;
  }
}
</style>

<div class="sample3">ホバーができるデバイスでは、ホバー時に背景が黒になる。</div>

{{</file>}}

```
<style>
.sample3 {
  background: lightgray;
  color: black;
}
.sample3:hover {
  background: yellow;
}

@media (hover: hover) {
  .sample3:hover {
    color: white;
    background: black;
  }
}

</style>

<div class="sample3">ホバーができるデバイスでは、ホバー時に背景が黒になる。</div>


```

##### 参考

{{<youtube oUNOGZP4PVg>}}
&nbsp;


{{<youtube oUNOGZP4PVg>}}
&nbsp;

https://www.w3schools.com/css/css_rwd_mediaqueries.asp

https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries

https://developer.mozilla.org/en-US/docs/Web/CSS/@media/hover

##### お薦め

{{<youtube _Rdwgjp8W88>}}
&nbsp;

{{<youtube MXcQdZu7Fdw>}}
&nbsp;

{{<youtube 1S7sU2wkmL4>}}
&nbsp;

{{<youtube ro9SyOrfWUk>}}
&nbsp;