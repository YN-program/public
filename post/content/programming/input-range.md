---
title: "【CSS】input type='range' のカスタマイズ"
description: ""
date: "2023-01-04T14:56:00+09:00"
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
input rangeを自作しました。

`::-webkit-slider-thumb`は非推奨のようなので、スライダーの"ポチ"も自作。

<!--more-->

{{<file>}}
<meta name="viewport" content="width=device-width">

<style>

.range_wrapper {
  width:70%;
  position:relative;
  margin:20px;
}
.sub_wrapper {
  display: block;
  width: 100%;
  height:50px;
  padding: 0px;
  text-decoration: none;
  background: ;
  touch-action:pan-x;
}
.range_balloon {
  position: absolute; 
  display: none;
  text-align:center;
  padding: 0 2px; 
  background-color: rgba(82, 82, 82, 0.5);
  color:#fff;
  width:40px;
  left : 47%;
  bottom : 100%;
  margin-bottom : 5px;
  font-size:;
  border-radius:7px;
}
.range_wrapper:hover .range_balloon,
.sub_wrapper:active .range_balloon {
  display:block;
}
.range_thumb {
  position: absolute; 
  display: block;
  text-align:center;
  background-color: #c1ffea;
  width:20px;
  height:20px;
  left : 50%;
  top : 4%;
  border-radius:50%;
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.2);
  cursor:pointer;
}

input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  cursor: pointer;
  outline: none;
  height: 14px;
  width: 100%;
  border: solid 2px #C6FFFF;
  border-radius:10px;
  content:"";
  padding:0;
}


input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none; ;
  background: #adffff;
  width: 0px;
  height: 0px;
  border-radius: 50%;
  content:"";
}
input[type="range"]::-moz-range-thumb {
  background:#adffff;
  width: 0px;
  height: 0px;
  border-radius: 50%;
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.2);
  border: none;
  content:"";
}
input[type="range"]::-moz-focus-outer {
  border: 0;
  content:"";
}
input[type="range"]:active::-webkit-slider-thumb {
  box-shadow: 0px 5px 10px -2px rgba(0, 0, 0, 0.3);
  content:"";
}


</style>
&nbsp;
<div class="range_wrapper">
  <span id="range_balloon" class="range_balloon"></span>
  <div id="sub_wrapper" class="sub_wrapper">
    <span id="range_thumb" class="range_thumb"></span>
    <input id="range" type="range" value="50" min="0" max="100">
  </div>
</div>

<script>

const range = document.getElementById("range"),
      text  = document.getElementById("range_balloon"),
      thumb = document.getElementById("range_thumb"),
      sub_wrapper = document.getElementById("sub_wrapper");
let value;
let mousePosition;
let x = 0;
let rect;
let mouseFlag = false;

const rate = Math.round(range.clientWidth / ( range.clientWidth + thumb.clientWidth) * 100) / 100;

let start = (e) =>{
  e.preventDefault();
  mouseFlag = true;
  rect = range.getBoundingClientRect();
  if (e.touches && e.touches[0]) {
    x = (e.touches[0].clientX - window.pageXOffset - rect.left) 
  } else if (e.clientX && e.clientY) {
    x =  (e.clientX - window.pageXOffset - rect.left)  //e.offsetX;
  }
  
  range.value = Math.round( x / range.clientWidth * 100 );
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
  thumb.style.left = (range.value - 0.5) * rate + "%";
  text.innerText  = range.value;
}

let move = (e) => {  
  e.preventDefault();
  rect = range.getBoundingClientRect();
  if (e.touches && e.touches[0]) {
    x = (e.touches[0].clientX - window.pageXOffset - rect.left) 
  } else if (e.clientX && e.clientY) {
    x = (e.clientX - window.pageXOffset - rect.left)   //e.offsetX;
  }
  if ( x < 0 ){
    mousePosition = 0
  } else if ( x > range.clientWidth){
    mousePosition = 100
  } else {
    mousePosition = Math.round(x / range.clientWidth * 100);
  }
  text.style.left = mousePosition + "%"
  text.innerText  = mousePosition;
  
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #ffffb2 ${mousePosition}%, #fff ${mousePosition}%, #fff 100%)`
  if (mouseFlag){
    range.value = mousePosition;
    thumb.style.left = (mousePosition - 0.5) * rate + "%";
  }
}

let end = (e) => {
  e.preventDefault();
  mouseFlag = false;
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
}

let range_input = (e) => {
  e.preventDefault();
  mouseFlag = false;
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
  text.style.left  = range.value + "%";
  text.innerText   = range.value;
  thumb.style.left = (range.value - 0.5) * rate + "%";
}

sub_wrapper.ontouchstart =
sub_wrapper.onmousedown  = start;

sub_wrapper.onmouseenter = 
sub_wrapper.ontouchmove  =
sub_wrapper.onmousemove  = move; 

sub_wrapper.ontouchend    =
sub_wrapper.ontouchcancel =
sub_wrapper.ontouchend    =
sub_wrapper.ontouchcancel =
sub_wrapper.onmouseleave  = end;

sub_wrapper.onclick   =
sub_wrapper.onmouseup =
range.oninput =
range.onchange = range_input;

range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
thumb.style.left = (range.value - 0.5) * rate + "%";


</script>

{{</file>}}



https://developer.mozilla.org/ja/docs/Web/CSS/::-webkit-slider-thumb


〇コード

```

<style>

.range_wrapper {
  width:70%;
  position:relative;
  margin:20px;
}
.sub_wrapper {
  display: block;
  width: 100%;
  height:50px;
  padding: 0px;
  text-decoration: none;
  background: ;
}
.range_balloon {
  position: absolute; 
  display: none;
  text-align:center;
  padding: 0 2px; 
  background-color: rgba(82, 82, 82, 0.5);
  color:#fff;
  width:40px;
  left : 47%;
  bottom : 100%;
  margin-bottom : 5px;
  font-size:;
  border-radius:7px;
}
.range_wrapper:hover .range_balloon {
  display:block;
}
.range_thumb {
  position: absolute; 
  display: block;
  text-align:center;
  background-color: #c1ffea;
  width:25px;
  height:25px;
  left : 50%;
  top : -3%;
  border-radius:50%;
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.2);
  cursor:pointer;
}

input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  cursor: pointer;
  outline: none;
  height: 14px;
  width: 100%;
  border: solid 2px #C6FFFF;
  border-radius:10px;
  content:"";
  padding:0;
}

/* ブラウザによってポチが見えるので、ブログではwidth:0; height:0;にしています。*/
/*
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none; ;
  background: #adffff;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  content:"";
}
input[type="range"]::-moz-range-thumb {
  background:#adffff;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.2);
  border: none;
  content:"";
}
input[type="range"]::-moz-focus-outer {
  border: 0;
  content:"";
}
input[type="range"]:active::-webkit-slider-thumb {
  box-shadow: 0px 5px 10px -2px rgba(0, 0, 0, 0.3);
  content:"";
}
*/


</style>

<div class="range_wrapper">
  <span id="range_balloon" class="range_balloon"></span>
  <div id="sub_wrapper" class="sub_wrapper">

    //range_thumb で本来のポチを隠しています。
    <span id="range_thumb" class="range_thumb"></span>
    <input id="range" type="range" value="50" min="0" max="100">
  </div>
</div>

<script>

const range = document.getElementById("range"),
      text  = document.getElementById("range_balloon"),
      thumb = document.getElementById("range_thumb"),
      sub_wrapper = document.getElementById("sub_wrapper");
let value;
let mousePosition;
let x = 0;
let rect;
let mouseFlag = false;

// あとで、スライダーのポチの位置を設定する際に rate を掛けます。
// これがないと、スライダーのポチがはみ出してしまいます。rate = 1;で確認できます。
const rate = Math.round(range.clientWidth / ( range.clientWidth + thumb.clientWidth) * 100) / 100;

let start = (e) =>{
  e.preventDefault();
  mouseFlag = true;
  rect = range.getBoundingClientRect();
  if (e.touches && e.touches[0]) {
    x = (e.touches[0].clientX - window.pageXOffset - rect.left) 
  } else if (e.clientX && e.clientY) {
    x =  (e.clientX - window.pageXOffset - rect.left)  //e.offsetX;
  }
  range.value = Math.round( x / range.clientWidth * 100 );
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
  thumb.style.left = range.value * rate + "%";
}

let move = (e) => {  
  e.preventDefault();
  rect = range.getBoundingClientRect();
  if (e.touches && e.touches[0]) {
    x = (e.touches[0].clientX - window.pageXOffset - rect.left) 
  } else if (e.clientX && e.clientY) {
    x = (e.clientX - window.pageXOffset - rect.left)   //e.offsetX;
  }
  if ( x < 0 ){
    mousePosition = 0
  } else if ( x > range.clientWidth){
    mousePosition = 100
  } else {
    mousePosition = Math.round(x / range.clientWidth * 100);
  }
  text.style.left = mousePosition + "%"
  text.innerText  = mousePosition;
  
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #ffffb2 ${mousePosition}%, #fff ${mousePosition}%, #fff 100%)`
  if (mouseFlag){
    range.value = mousePosition;
    thumb.style.left = mousePosition * rate + "%";
  }
}

let end = (e) => {
  e.preventDefault();
  mouseFlag = false;
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
}

let range_input = (e) => {
  e.preventDefault();
  mouseFlag = false;
  range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
  text.style.left  = range.value + "%";
  text.innerText   = range.value;
  thumb.style.left = range.value * rate + "%";
}

sub_wrapper.ontouchstart =
sub_wrapper.onmousedown  = start;

sub_wrapper.onmouseenter = 
sub_wrapper.ontouchmove  =
sub_wrapper.onmousemove  = move; 

sub_wrapper.ontouchend    =
sub_wrapper.ontouchcancel =
sub_wrapper.onmouseleave  = end;

sub_wrapper.onclick   =
sub_wrapper.onmouseup =
range.oninput  =
range.onchange = range_input;

range.style.background = `linear-gradient(to right, #ffffe0 0%, #ffffb2 ${range.value}%, #fff ${range.value}%, #fff 100%)`
thumb.style.left = range.value * rate + "%";


</script>


```


スライダーを縦方向にする方法。

> Chrome, Opera, Safari の場合
> ```
> input[type="range"] {
>   -webkit-appearance:slider-vertical;
> }
>  ```
> 
> IE, Edge の場合
> ```
> input[type="range"] {
>   writing-mode: bt-lr;
> }
> ``` 
> 
> Firefox の場合
> orient 属性に vertical を指定します。
> 
> ```
> <input type="range" name="range" orient="vertical">
> ```
>
> ```
> <input type="range" name="range" min="0" max="100" value="50" orient="vertical">
> <style type="text/css">
>   input[type="range"] {
>     -webkit-appearance:slider-vertical;
>     writing-mode: bt-lr;
>     height:50px;
>   }
> </style>
> ```
>
> https://codeforfun.jp/reference-html-tag-input-type-range/



##### 〇 Javascriptのメモ

〇[mouseleave イベント](https://developer.mozilla.org/ja/docs/Web/API/Element/mouseleave_event)は、ポインティングデバイス (ふつうはマウス) のカーソルが要素 (Element) の外に移動したときに発行される。
* バブリングなし
* キャンセル不可
* `mouseleave`はバブリングしないのに対して、`mouseout`はバブリングする。

〇[mouseover イベント](https://developer.mozilla.org/ja/docs/Web/API/Element/mouseover_event)は、ポインティングデバイス (マウスやトラックパッドなど) のカーソルが要素またはその子要素のうちの一つの上を移動したときに、その要素 (Element) に発行される。
* バブリングあり
* キャンセル可

〇[mouseenter イベント](https://developer.mozilla.org/ja/docs/Web/API/Element/mouseenter_event)は、ポインティングデバイス (通常はマウス) のホットスポットが最初にイベントが発行された要素の中に移動したときにその要素 (Element) に発行される。
* バブリングなし
* キャンセル不可

〇タッチされた画面の位置取得

>```
> const rect = event.target.getBoundingClientRect()
> const offsetX = (event.touches[0].clientX - window.pageXOffset - rect.left) 
> const offsetY = (event.touches[0].clientY - window.pageYOffset - rect.top)
> ```
> [[JavaScript]タッチイベントからoffsetXとoffsetXを取得する](https://qiita.com/yumiya_yumigon/items/6e7548e14f11dd8a86f0)

> ```
> 
> let x = 0, y = 0;
> 
> if (e.touches && e.touches[0]) {
> 
>   x = e.touches[0].clientX;
>   y = e.touches[0].clientY;
> 
> }
> else if (e.clientX && e.clientY) {
> 
>   x = e.clientX;
>   y = e.clientY;
> 
> }
> ```
> [javascript タッチされた画面の位置を取得する](https://mebee.info/2020/09/02/post-17902/)

##### お薦め

{{<youtube 1S7sU2wkmL4>}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1610954519470563328">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1610228514942509057">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1609870250467799044">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1609144518527574020">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1608806948488761350">}}
&nbsp;