---
title: "【CSS】要素幅、高さをコントロールするプロパティのメモ"
description: ""
date: "2023-01-08T11:22:46+09:00"
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
`max-content`, `min-content`, `fit-content`, `min-width`, `max-width`, `min-height`, `max-height`, `vw`, `vh`, `vmax`, `vmin`,`aspect-ratio`についてのメモです。

<!--more-->


`max-content`
* コンテンツの本質的な最大幅または最大高さを表す。テキストコンテンツの場合、コンテンツがオーバーフローを起こしても折り返されない。
* `margin:0 auto;`で要素を中央寄せできる。

https://developer.mozilla.org/en-US/docs/Web/CSS/max-content

{{<file>}}

<style>
#blog-container {
  background: #f7f7f7;
  padding: 1rem;
  width: 20rem;
}

.blog-item {
  width: max-content;
  background: lightgray;
  padding: 0.5rem;
  margin-bottom: 1rem;
  /* margin:0 auto; */
  border-bottom:2px solid black;
}

#blog2-container {
  display: grid;
  grid-template-columns: max-content max-content 1fr;
  grid-gap: 0.5rem;
  box-sizing: border-box;
  height: 10rem;
  width: 100%;
  background: #f7f7f7;
  padding: 1rem;
}

#blog2-container > div {
  background: lightgray;
  padding: 0.5rem;
}

</style>

<div id="blog-container">
  <div class="blog-item">要素</div>
  <div class="blog-item">
    widthにmax-contentを指定しているので、テキストが折り返されずに親要素からはみ出す。
  </div>
</div>

<br>
<div id="blog2-container">
  <div>要素</div>
  <div>長いテキスト。長いテキスト。</div>
  <div>フレキシブルな要素</div>
</div>


{{</file>}}
&nbsp;

```
<style>
#blog-container {
  background: #f7f7f7;
  padding: 1rem;
  width: 20rem;
}

 /* margin:0 auto; で要素が中央にくる。*/
 /* borderは要素幅になる。*/
.blog-item {
  width: max-content;
  background: lightgray;
  padding: 0.5rem;
  margin-bottom: 1rem;
   /* margin:0 auto; */
  border-bottom:2px solid black;
}

#blog2-container {
  display: grid;
  grid-template-columns: max-content max-content 1fr;
  grid-gap: 0.5rem;
  box-sizing: border-box;
  height: 10rem;
  width: 100%;
  background: #f7f7f7;
  padding: 1rem;
}

#blog2-container > div {
  background: lightgray;
  padding: 0.5rem;
}

</style>

<div id="blog-container">
  <div class="blog-item">要素</div>
  <div class="blog-item">
    widthにmax-contentを指定しているので、テキストが折り返されずに親要素からはみ出す。
  </div>
</div>

<br>
<div id="blog2-container">
  <div>要素</div>
  <div>長いテキスト。長いテキスト。</div>
  <div>フレキシブルな要素</div>
</div>
```

grid-template-columnsについては[こちら](https://www.w3schools.com/cssref/pr_grid-template-columns.php)


`min-content`
* コンテンツの内在的な最小幅を表す。テキストでは最も長い単語の幅と等しくなる。

https://developer.mozilla.org/ja/docs/Web/CSS/min-content

{{<file>}}

<style>

#min-container {
  background: #f7f7f7;
  padding: 1rem;
  width: 20rem;
}
.min-item {
  width: min-content;
  background: lightgray;
  padding: 0.5rem;
  margin-bottom: 1rem;
}

#min2-container {
  display: grid;
  grid-template-columns: min-content min-content min-content 1fr;
  grid-gap: 0.5rem;
  box-sizing: border-box;
  height: 10rem;
  width: 100%;
  background: #f7f7f7;
  padding: 1rem;
}

#min2-container > div {
  background: lightgray;
  padding: 0.5rem;
}

</style>
<div id="min-container">
  <div class="min-item">要素</div>
  <div class="min-item">widthにmin-contentを指定している。</div>
</div>

<br>
<div id="min2-container">
  <div>要素</div>
  <div>アイテム</div>
  <div>abcdefghijklmn</div>
  <div>grid-template-columns: min-content min-content min-content 1fr; 高さが親要素をはみ出す可能性あり。</div>
</div>


{{</file>}}
&nbsp;

```

<style>

#min-container {
  background: #f7f7f7;
  padding: 1rem;
  width: 20rem;
}
.min-item {
  width: min-content;
  background: lightgray;
  padding: 0.5rem;
  margin-bottom: 1rem;
}

#min2-container {
  display: grid;
  grid-template-columns: min-content min-content min-content 1fr;
  grid-gap: 0.5rem;
  box-sizing: border-box;
  height: 10rem;
  width: 100%;
  background: #f7f7f7;
  padding: 1rem;
}

#min2-container > div {
  background: lightgray;
  padding: 0.5rem;
}

</style>
<div id="min-container">
  <div class="min-item">要素</div>
  <div class="min-item">widthにmin-contentを指定している。</div>
</div>

<br>
<div id="min2-container">
  <div>要素</div>
  <div>アイテム</div>
  <div>abcdefghijklmn</div>
  <div>grid-template-columns: min-content min-content min-content 1fr; 高さが親要素をはみ出す可能性あり。</div>
</div>

```

`fit-content`
* 画面の幅に応じて、max-content, min-contentを適用したい場合は、fit-contentを使用する。

https://developer.mozilla.org/ja/docs/Web/CSS/fit-content

{{<file>}}
<style>
.fit-container {
  padding: 1rem;
  width: 100%;
  background:#f7f7f7
}

.fit-item {
  width: -moz-fit-content;
  width: fit-content;
  background: lightgray;
  padding: 0.5rem;
  margin-bottom: 1rem;
}
</style>

<div class="fit-container">
  <div class="fit-item">要素</div>
  <div class="fit-item">長いテキスト</div>
  <div class="fit-item">fit-contentを適用。画面幅が狭くなると、テキストは折り返される。</div>
</div>

{{</file>}}

```
<style>
.fit-container {
  padding: 1rem;
  width: 100%;
  background:#f7f7f7
}

.fit-item {
  width: -moz-fit-content;
  width: fit-content;
  background: lightgray;
  padding: 0.5rem;
  margin-bottom: 1rem;
}
</style>

<div class="fit-container">
  <div class="fit-item">要素</div>
  <div class="fit-item">長いテキスト</div>
  <div class="fit-item">fit-contentを適用。画面幅が狭くなると、テキストは折り返される。</div>
</div>
```

***


`vw`: viewport widthの略。1vw はviewportの幅の1%。viewportの幅は1000pxであれば、1vwは10pxになる。

`vh`: viewport heightの略。1vh はviewportの高さの1%。
* メモ：height にvwを指定することで、要素の比率を変えずにサイズを変化させることができる。

`vmin`: viewport minimumの略。viewportの高さと幅の小さいほうのサイズに基づく。幅＞高さの場合、1vminはviewportの高さの1%に、幅＜高さの場合、1vminはviewportの幅の1%に相当する。
`height: 80vmin;`のように指定する。

`vmax`: viewport maximumの略。viewportの高さと幅の大きいほうのサイズに基づく。幅＞高さの場合、1vminはviewportの幅の1%に、幅＜高さの場合、1vminはviewportの高さの1%に相当する。
`height: 80vmax;`のように指定する。

viewportは、webサイトの表示領域のこと。

***

`min-width`
* 要素の最小幅を定義する。
* コンテンツが最小幅より小さい場合、最小幅が適用される。
* コンテンツが最小幅より大きい場合、min-widthプロパティは影響を与えない。
* widthプロパティの値がmin-widthより小さくなるのを防ぐためのもの。

`max-width`
* 要素の最大幅を定義する。
* コンテンツが最大幅より大きい場合、要素の高さが自動的に変更されます。
* コンテンツが最大幅より小さい場合、max-width プロパティは何の効果もない。
* max-widthプロパティの値は、widthプロパティより優先される。

`min-height`
* 要素の高さの最小値を定義する。
* コンテンツが最小の高さより小さい場合、最小の高さが適用される。
* コンテンツが最小の高さより大きい場合、min-height プロパティは影響を及ぼさない。
* heightプロパティの値がmin-heightより小さくなるのを防ぐためのもの。

`max-height`
* 要素の最大高さを定義する
* コンテンツが最大高さより大きい場合、コンテンツはオーバーフローする。コンテナが溢れた内容をどのように処理するかは、overflow プロパティで定義される。
* コンテンツが最大高さより小さい場合、max-height プロパティは何の影響も及ぼさない。
* max-heightプロパティの値は、heightプロパティより優先されます。

`aspect-ratio`
* 要素の幅と高さの比率を指定することができる。
* 要素に`width`が指定されていると、高さは指定されたアスペクト比に従う。


```
aspect-ratio:auto;

aspect-ratio: 1/1;

aspect-ratio:16/9;

aspect-ratio:0.5;

aspect-ratio:2;

```

##### お薦め

NANAさんの歌　「もうひとつの実を望まれ」

最高に癒される、超おすすめの歌です。世界中の全ての方に歌ってほしい歌です。是非、一緒に歌ってみてください。

{{<youtube _Rdwgjp8W88>}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1612058290216923137">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1612055127581683712">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1612042758218473473">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1612031051223945218">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1611690904271880192">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1611680868350980097">}}
&nbsp;
