---
title: "Inputタグを使わないrange slider"
description: ""
date: "2023-01-20T16:38:52+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "CSS"
  - "Javascript"
menu:
main:
  name: プログラミング
  weight: 2
---
inputタグを使わないレンジスライダーを自作しました。


&nbsp;
{{<file>}}

<style>

.slider_wrapper {
    width:100%;
    height:30px;
    background:rgba(246,246,255,1);
    border-radius:15px;
    position:relative;
    box-shadow:2px 5px 3px gray;
}
.slider_subwrapper {
    width:90%;
    height:30px;
    display:block;
    position:relative;
    margin:0 auto;
    cursor:pointer;
    background:inherit;
    z-index:200;
}
.slider_baloon {
    position: absolute;
    display: none;
    text-align:center;
    padding: 0 2px;
    background-color: rgba(82, 82, 82, 0.5);
    color:#fff;
    left : 5%;
    bottom : 100%;
    margin-bottom : 5px;
    font-size:;
    border-radius:7px;
}
.slider_wrapper:hover .slider_baloon {
    display:block;
}
.slider_thumb {
    position: absolute;
    display: block;
    text-align:center;
    background: rgba(163,163,255,1);
    width:12px;
    height:12px;
    left : 0%;
    top : 10px;
    border-radius:50%;
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.2);
    cursor:pointer;
    z-index:100;
}
.slider {
    width:100%;
    height:4px;
    background:#fff;
    cursor:pointer;
    position:absolute;
    top:13px;
    left:0%;
}
.slider_progress {
    height:4px;
    background: rgba(163,163,255,1);
    cursor:pointer;
}

</style>
<body>


<div id="slider_wrapper"  class="slider_wrapper">
    <div id="slider_subwrapper" class="slider_subwrapper">
        <span id="slider_baloon" class="slider_baloon"></span>
        <div id="slider" class="slider" role="slider">
            <div id="progress_slider"></div>
        </div>
        <span id="slider_thumb" class="slider_thumb"></span>
    </div>
</div>


</body>

<script>

const progressSlider    = document.getElementById("progress_slider"),
      slider            = document.getElementById("slider"),
      sliderSubWrapper  = document.getElementById("slider_subwrapper"),
      sliderThumb       = document.getElementById("slider_thumb"),
      text              = document.getElementById("slider_baloon");

let WidthX, rect, thumbRate;
let x = 0;
let mousedownFlag = false;

sliderSubWrapper.ontouchstart =
sliderSubWrapper.onmousedown  = mousedownSlider;

sliderSubWrapper.ontouchend    =
sliderSubWrapper.onmouseup     =
sliderSubWrapper.onmouseleave  =
sliderSubWrapper.ontouchcancel = mouseupSlider;

sliderSubWrapper.onmouseenter =
sliderSubWrapper.ontouchmove  =
sliderSubWrapper.onmousemove  = moveSlider;


function mouseX(e) {
    rect = sliderSubWrapper.getBoundingClientRect();
    if (e.touches && e.touches[0]) {
        x = (e.touches[0].clientX - window.pageXOffset - rect.left)
    } else if (e.clientX && e.clientY) {
        x = (e.clientX - window.pageXOffset - rect.left) //e.offsetXではマウスがthumbの上にあるときにうまく行かない。
    }
    if ( x < 0 ){
        WidthX = 0
    } else if ( x > sliderSubWrapper.clientWidth){
        WidthX = 100
    } else {
        WidthX = Math.round(x / sliderSubWrapper.clientWidth * 100);
    }
    return WidthX;
}
function setProgressSlider (WidthX) {
    progressSlider.style.width =  WidthX + '%';
    progressSlider.classList.add("slider_progress");
    sliderThumb.style.left = WidthX * thumbRate + "%";
}
function mousedownSlider(e){
    e.preventDefault();
    slider.style.background = `#fff`;
    mousedownFlag = true;
    WidthX = mouseX(e);
    setProgressSlider(WidthX);
    text.style.left = WidthX + "%";
    text.innerText  = WidthX + "%";
}
function mouseupSlider(e){
    e.preventDefault();
    if (mousedownFlag){
        mousedownFlag = false;
        setProgressSlider(WidthX);
    }else{
        slider.style.background = `#fff`;
        mousedownFlag = false;
    }
}
function moveSlider(e){
    e.preventDefault();
    WidthX = mouseX(e);
    text.style.left = WidthX + "%";
    text.innerText  = WidthX + "%";
    if(mousedownFlag){
        setProgressSlider(WidthX);
    } else {
        slider.style.background = `linear-gradient(to right, rgba(163,163,255,0.4) 0%, rgba(163,163,255,0.4) ${WidthX}%, #fff ${WidthX}%, #fff 100%)`
    }
}

thumbRate = sliderSubWrapper.clientWidth / ( sliderSubWrapper.clientWidth + sliderThumb.clientWidth);


</script>


{{</file>}}


<!--more-->

`e.preventDefault();`で、touchイベントの際に、mouseイベントが発生しないようにしています。

`thumbRate`で、スライダーが100%の時に、’ポチ’が右にはみ出さないようにしています。


```

<style>

.slider_wrapper {
    width:100%;
    height:30px;
    background:rgba(246,246,255,1);
    border-radius:15px;
    position:relative;
    box-shadow:2px 5px 3px gray;
}
.slider_subwrapper {
    width:90%;
    height:30px;
    display:block;
    position:relative;
    margin:0 auto;
    cursor:pointer;
    background:inherit;
    z-index:200;
}
.slider_baloon {
    position: absolute;
    display: none;
    text-align:center;
    padding: 0 2px;
    background-color: rgba(82, 82, 82, 0.5);
    color:#fff;
    left : 5%;
    bottom : 100%;
    margin-bottom : 5px;
    font-size:;
    border-radius:7px;
}
.slider_wrapper:hover .slider_baloon {
    display:block;
}
.slider_thumb {
    position: absolute;
    display: block;
    text-align:center;
    background: rgba(163,163,255,1);
    width:12px;
    height:12px;
    left : 0%;
    top : 10px;
    border-radius:50%;
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.2);
    cursor:pointer;
    z-index:100;
}
.slider {
    width:100%;
    height:4px;
    background:#fff;
    cursor:pointer;
    position:absolute;
    top:13px;
    left:0%;
}
.slider_progress {
    height:4px;
    background: rgba(163,163,255,1);
    cursor:pointer;
}

</style>

<body>

    <div id="slider_wrapper"  class="slider_wrapper">
        <div id="slider_subwrapper" class="slider_subwrapper">
            <span id="slider_baloon" class="slider_baloon"></span>
            <div id="slider" class="slider" role="slider">
                <div id="progress_slider"></div>
            </div>
            <span id="slider_thumb" class="slider_thumb"></span>
        </div>
    </div>

</body>

<script>

const progressSlider    = document.getElementById("progress_slider"),
      slider            = document.getElementById("slider"),
      sliderSubWrapper  = document.getElementById("slider_subwrapper"),
      sliderThumb       = document.getElementById("slider_thumb"),
      text              = document.getElementById("slider_baloon");

let WidthX, rect, thumbRate;
let x = 0;
let mousedownFlag = false;

sliderSubWrapper.ontouchstart =
sliderSubWrapper.onmousedown  = mousedownSlider;

sliderSubWrapper.ontouchend    =
sliderSubWrapper.onmouseup     =
sliderSubWrapper.onmouseleave  =
sliderSubWrapper.ontouchcancel = mouseupSlider;

sliderSubWrapper.onmouseenter =
sliderSubWrapper.ontouchmove  =
sliderSubWrapper.onmousemove  = moveSlider;


function mouseX(e) {
    rect = sliderSubWrapper.getBoundingClientRect();
    if (e.touches && e.touches[0]) {
        x = (e.touches[0].clientX - window.pageXOffset - rect.left)
    } else if (e.clientX && e.clientY) {
        x = (e.clientX - window.pageXOffset - rect.left) 
        // x = e.offsetX ではマウスがthumbの上にあるときにうまく行かない。
    }
    if ( x < 0 ){
        WidthX = 0
    } else if ( x > sliderSubWrapper.clientWidth){
        WidthX = 100
    } else {
        WidthX = Math.round(x / sliderSubWrapper.clientWidth * 100);
    }
    return WidthX;
}
function setProgressSlider (WidthX) {
    progressSlider.style.width =  WidthX + '%';
    progressSlider.classList.add("slider_progress");
    sliderThumb.style.left = WidthX * thumbRate + "%";
}
function mousedownSlider(e){
    e.preventDefault();
    slider.style.background = `#fff`;
    mousedownFlag = true;
    WidthX = mouseX(e);
    setProgressSlider(WidthX);
    text.style.left = WidthX + "%";
    text.innerText  = WidthX + "%";
}
function mouseupSlider(e){
    e.preventDefault();
    if (mousedownFlag){
        mousedownFlag = false;
        setProgressSlider(WidthX);
    }else{
        slider.style.background = `#fff`;
        mousedownFlag = false;
    }
}
function moveSlider(e){
    e.preventDefault();
    WidthX = mouseX(e);
    text.style.left = WidthX + "%";
    text.innerText  = WidthX + "%";
    if(mousedownFlag){
        setProgressSlider(WidthX);
    } else {
        slider.style.background = `linear-gradient(to right, rgba(163,163,255,0.4) 0%, rgba(163,163,255,0.4) ${WidthX}%, #fff ${WidthX}%, #fff 100%)`
    }
}

thumbRate = sliderSubWrapper.clientWidth / ( sliderSubWrapper.clientWidth + sliderThumb.clientWidth);


</script>

```

##### 参考記事
[[JavaScript]タッチイベントからoffsetXとoffsetXを取得する](https://qiita.com/yumiya_yumigon/items/6e7548e14f11dd8a86f0)

[【JavaScript】click と touchend の両方のイベントリスナーのうち touchend だけを実行する
](https://webfrontend.ninja/js-click-and-touchend-events/)


##### お薦め

{{<tweet user="Rapt_plusalpha" id="1616765149142216706">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1616753589929807873">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1616742790943375367">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1616739903823577090">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1616412297890758658">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1616397839638364161">}}
&nbsp;

{{<tweet user="Rapt_plusalpha" id="1616046859377864705">}}
&nbsp;