---
title: "【Javascript】オーディオタグのカスタマイズ"
description: ""
date: "2022-12-18T14:38:30+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Javascript"
menu:
main:
  name: プログラミング
  weight: 2
---

オーディオタグを Javascript で自作してみました。


<!--more-->

(追記：iosでのボタンの表記が乱れています。

またiOSではJSでのボリューム変更ができないようです（[こちら](https://mizominton.hatenablog.jp/entry/ios-safari-volume)）。)

(追記：2022/12/22　javascript では、iOSでボリューム変更ができないようなので、

ブログ用にjavascriptに以下のコードを追加して、iOSでのボリューム操作のボタンを隠しています。

web audio APIを使うと出来そうですが、試行錯誤中です。）

```

    const ua = navigator.userAgent.toLowerCase();
    const isSafari = (ua.indexOf('safari') > -1) && (ua.indexOf('chrome') == -1);
    const isiPhone = (ua.indexOf('iphone') > -1);
    const isiPad = (ua.indexOf('ipad') > -1);
    if (isSafari || isiPhone || isiPad){
        volUpButton.classList.add("d-none");
        volDownButton.classList.add("d-none");
    }
```

{{<file>}}

<style>
/* body { -webkit-text-size-adjust: 100％; } */
.p {
    overflow: hidden;
    white-space: nowrap;
    margin:0;
  }
audio {
    width:100%;
}
.audio_area {
    display:flex;
    flex-wrap:wrap;
    padding:0.25rem 0.5rem;
}
.audio_btn {
    font-size:0.8rem;
    width:3rem;
    height:3rem;
    text-align:center;
    color:#fff;
    background: rgba(254, 220, 64, 1);
    background-image: linear-gradient(90deg, rgba(247, 93, 139, 1), rgba(254, 220, 64, 1));
    border:none;
    border-radius:50%;
    margin:0.25rem;
    -webkit-appearance: none;
}
.audio_btn:hover{
    background-image: linear-gradient(45deg, rgba(247, 93, 139, 1), rgba(254, 220, 64, 1));
    color:#424242;
}
.audio_text {
    font-size:1.2rem;
}
.normal-line-height {line-height:normal;}
.audio_skip_forward {
    position: relative;
    margin-left:0.7rem;
    color: #fff;
    font-size:1.4rem;
    transform:rotate(180deg);
}
.audio_skip_backward {
    position: relative;
    margin-left:0.6rem;
    color: #fff;
    font-size:1.4rem;
    transform:scale(1,-1);
}
.audio_wrapper {
    background: inherit;
    width:100%;
    padding:0.25rem 0;
    border:solid 1px rgba(255, 223, 74, 1);
    border-radius:0.25rem;
    height:auto;
}

.audio_time_bar_area {
    width:100%;
    height:1rem;
    background:#f2f2f2;
    cursor:pointer;
}
.audio_time_bar{
    height:1rem;
    background-image: linear-gradient(90deg, rgba(255, 146, 179, 1), rgba(255, 223, 74, 1));
    cursor:pointer;
}
.d-none { display:none;}
.text-center {text-align:center;}
.container { padding:0.5rem;}
.row {display:flex;}
.col8{width:80%;}
.col4{width:20%;}
</style>


<audio id="audio" src="/img/audio/MusMus-BGM-154.mp3" type="audio/mp3"></audio>

<button type="button" id="start_button" class="audio_btn startAudio">play</button>

<div id="wrapper" class="audio_wrapper d-none">
    <div class=audio_area>
        <button type="button" id="stop_button" class="audio_btn"><p class="p">stop</p></button>
        <button type="button" id="pause_button" class="audio_btn normal-line-height"><p class="p">pause</p></button>
        <button type="button" id="skip_backward" class="audio_btn normal-line-height"><p class="p">skip</p><p class="p">-10</p></button>
        <button type="button" id="skip_forward" class="audio_btn normal-line-height"><p class="p">skip</p><p class="p">+10</p></button>
        <button type="button" id="vol_up_btn" class="audio_btn normal-line-height"><p class="p">vol</p><p class="p">+</p></button>
        <button type="button" id="vol_down_btn" class="audio_btn normal-line-height"><p class="p">vol</p><p class="p">-</p></button>
        <button type="button" id="muteBtn" class="audio_btn"><p class="p">Vol</p></button>
        <button type="button" id="rateUp" class="audio_btn normal-line-height">
          <p class="p">rate</p><p class="p">+</p>
        </button>
        <button type="button" id="rateDown" class="audio_btn normal-line-height">
          <p class="p">rate</p><p class="p">-</p>
        </button>
    </div>
    <div class="w-100">
        <div id="volText" class="text-center d-none"></div>
        <div id="audioRate" class="text-center d-none"></div>
    </div>
    <div class="container">
        <div class="row">
            <div id="timeSlider" class="audio_time_bar_area col8">
                <div id="audio_time_bar"></div>
            </div>
            <div class="text-center col4">
                <span id="minutes">00</span>:<span id="seconds">00</span>
                /<span id="audioMinutes"></span>:<span id="audioSeconds"></span>
            </div>
        </div>
    </div>
</div>

<script>

window.addEventListener("load" , function (){
    let startBtn = document.getElementById("start_button");
    startBtn.onclick = setAudio;
    // startBtn.onclick = () => {
    //     const audio = document.getElementById('audio');
    //     audio.currentTime = 0;
    //     setAudio();
    // }
});

function setAudio(){

    const audio = document.getElementById('audio');
    audio.currentTime = 0;
    const wrapper       = document.getElementById('wrapper' );
    wrapper.classList.remove("d-none");
    const startButton   = document.getElementById('start_button' );
    const stopButton    = document.getElementById('stop_button');
    const pauseButton   = document.getElementById('pause_button');
    const skipForward   = document.getElementById('skip_forward');
    const skipBackward  = document.getElementById('skip_backward');
    const volUpButton   = document.getElementById('vol_up_btn');
    const volDownButton = document.getElementById('vol_down_btn');
    const volText       = document.getElementById('volText');
    const muteBtn       = document.getElementById('muteBtn');
    const rateUp        = document.getElementById('rateUp');
    const rateDown      = document.getElementById('rateDown');
    const audioMinutes  = document.getElementById("audioMinutes");
    const audioSeconds  = document.getElementById("audioSeconds");
    const audioRateTxt  = document.getElementById("audioRate");

    var minutes         = document.getElementById("minutes");
    var seconds         = document.getElementById("seconds");
    var audioTime       = 0;
    var audioTimeBar    = document.getElementById("audio_time_bar");
    var slider          = document.getElementById('timeSlider');

    let paused     = false;
    let changeRate = false;
    let interval;

    function pad(val) { return val > 9 ? val : "0" + val; }
    audioMinutes.innerHTML = pad(Math.floor(audio.duration / 60));
    audioSeconds.innerHTML = pad(Math.floor(audio.duration % 60));

    startButton.classList.add("d-none");

    const ua = navigator.userAgent.toLowerCase();
    const isSafari = (ua.indexOf('safari') > -1) && (ua.indexOf('chrome') == -1);
    const isiPhone = (ua.indexOf('iphone') > -1);
    const isiPad = (ua.indexOf('ipad') > -1);
    if (isSafari || isiPhone || isiPad){
        volUpButton.classList.add("d-none");
        volDownButton.classList.add("d-none");
    }

    if( audio.volume === 0){
        muteBtn.innerHTML = 'Mute';
    }

    audio.onended = () => {
        stopAudio();
        paused = true;
    }
    stopButton.onclick = (e) => {
        e.stopPropagation();
        audio.pause();
        stopAudio();
        paused = true;
    }
    pauseButton.onclick = (e) => {
        e.stopPropagation();
        if ( paused ) {
            audio.play();
            pauseButton.innerHTML = 'pause';
            paused = false;
            audioCounter();
        } else {
            audio.pause();
            pauseButton.innerHTML = 'play';
            paused = true;
            clearInterval(interval);
            delete interval;
        }
    }
    skipForward.onclick = (e) => {
        e.stopPropagation();
        audioTime += 10;
        if (audioTime > audio.duration){
            audioTime = audio.duration;
        }
        audio.currentTime = audioTime;
        setCounter();
    }
    skipBackward.onclick = (e) => {
        e.stopPropagation();
        audioTime -= 10;
        if (audioTime < 0){
            audioTime = 0;
        }
        audio.currentTime = audioTime;
        setCounter();
    }
    volUpButton.onclick = (e) => {
        e.stopPropagation();
        if(audio.volume <= 0.9){
        audio.volume += 0.1;
        volText.textContent = "vol : " + Math.round((audio.volume * 10)) / 10;
        audioText(volText);
        }else{;}
    }
    volDownButton.onclick = (e) => {
        e.stopPropagation();
        if(audio.volume >= 0.1){
            audio.volume -= 0.1;
            volText.textContent = "vol : " + Math.round((audio.volume * 10)) / 10;
            audioText(volText);
        }else{
            audio.volume = 0;
            volText.textContent = "vol : " + 0;
            audioText(volText);
        }
    }
    muteBtn.onclick = (e) => {
        e.stopPropagation();
        if( audio.muted ){
            muteBtn.innerHTML = 'Vol';
            audio.muted = false;
            volText.textContent = "vol : " + Math.round((audio.volume * 10)) / 10;
            audioText(volText);
        } else {
            muteBtn.innerHTML = 'Mute';
            audio.muted = true;
            volText.textContent = "vol : " + 0;
            audioText(volText);
        }
    }
    rateUp.onclick = (e) => {
        e.stopPropagation();
        audio.playbackRate += 0.1;
        audioRateTxt.textContent = Math.round((audio.playbackRate * 10)) / 10 + "×";
        audioText(audioRateTxt);
    }
    rateDown.onclick = (e) => {
        e.stopPropagation();
        if(audio.playbackRate > 0.1){
            audio.playbackRate -= 0.1;
        }
        audioRateTxt.textContent = Math.round((audio.playbackRate * 10)) / 10  + "×";
        audioText(audioRateTxt);
    }
    slider.onclick = (e) => {
        e.stopPropagation();
//        var click_x = e.pageX ;
//        var client_rect = slider.getBoundingClientRect();
//        var position_x = client_rect.left + window.pageXOffset;
//        var x = click_x - position_x ;
//        audioTime = Math.round( audio.duration * (x / client_rect.width) );
        audioTime = Math.round( audio.duration * e.offsetX /  slider.clientWidth)
        audio.currentTime = audioTime;
        setCounter();
    }

    function stopAudio() {
        startButton.classList.remove("d-none");
        minutes.innerHTML = "00";
        seconds.innerHTML = "00";
        wrapper.classList.add("d-none");
        if( paused ){
            pauseButton.innerHTML = 'pause';
        }
    }
    function setAudioTimeBar () {
        audio.ontimeupdate = () =>{
            var Width = (audio.currentTime / audio.duration * 100) + '%';
            audioTimeBar.style.width = Width;
            audioTimeBar.classList.add("audio_time_bar");
        }
    }
    function audioCounter(){
        let interval = setInterval( function(){
            if (!paused){
                audioTime += audio.playbackRate;
                setCounter();
            }else{
                clearInterval(interval);
                delete interval;
            }
        }, 1000);
        console.log("delete")
    }
    function setCounter(){
        minutes.innerHTML = pad(Math.floor(audioTime / 60));
        seconds.innerHTML = pad(Math.floor(audioTime % 60));

    }
    function audioText(elem){
        elem.classList.remove("d-none");
        var showText = setTimeout( function(){
            elem.classList.add("d-none");
            clearTimeout( showText );
        }, 700);
    }
    audioCounter();
    audio.play();
    setAudioTimeBar();
}

</script>

{{</file>}}

*音量注意

音楽サンプルは「[フリーBGM・音楽素材MusMus](https://musmus.main.jp/music_img2.html)」から

template は、django/python 環境下で、forloop内のオーディオタグに対して。

一部にbootstrapとfont awesome使用しています。

`template`

```

<audio id="audio_{{ forloop.counter }}" src=""></audio>
<button type="button" id="start_button_{{ forloop.counter }}" class="audio_btn startAudio"><i class="fas fa-play"></i></button>

<div id="wrapper_{{ forloop.counter }}" class="audio_wrapper d-none">
    <div class=audio_area>
        <button type="button" id="stop_button_{{ forloop.counter }}" class="audio_btn">
          <i class="fas fa-stop"></i>
        </button>
        <button type="button" id="pause_button_{{ forloop.counter }}" class="audio_btn">
          <i class="fas fa-pause"></i>
        </button>
        <button type="button" id="skip_backward_{{ forloop.counter }}" class="audio_btn position-relative">
            <svg class="audio_skip_backward" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="_x32_" x="0px" y="0px" viewBox="0 0 512 512" style="width:1.2rem;" xml:space="preserve">
            <style type="text/css">
                .st0{fill:#4B4B4B;}
            </style>
            <g>
                <path class="st0" d="M446.025,92.206c-40.762-42.394-97.487-69.642-160.383-72.182c-15.791-0.638-29.114,11.648-29.752,27.433   c-0.638,15.791,11.648,29.114,27.426,29.76c47.715,1.943,90.45,22.481,121.479,54.681c30.987,32.235,49.956,75.765,49.971,124.011   c-0.015,49.481-19.977,94.011-52.383,126.474c-32.462,32.413-76.999,52.368-126.472,52.382   c-49.474-0.015-94.025-19.97-126.474-52.382c-32.405-32.463-52.368-76.992-52.382-126.474c0-3.483,0.106-6.938,0.302-10.364   l34.091,16.827c3.702,1.824,8.002,1.852,11.35,0.086c3.362-1.788,5.349-5.137,5.264-8.896l-3.362-149.834   c-0.114-4.285-2.88-8.357-7.094-10.464c-4.242-2.071-9.166-1.809-12.613,0.738L4.008,182.45c-3.05,2.221-4.498,5.831-3.86,9.577   c0.61,3.759,3.249,7.143,6.966,8.974l35.722,17.629c-1.937,12.166-3.018,24.602-3.018,37.279   c-0.014,65.102,26.475,124.31,69.153,166.944C151.607,465.525,210.8,492.013,275.91,492   c65.095,0.014,124.302-26.475,166.937-69.146c42.678-42.635,69.167-101.842,69.154-166.944   C512.014,192.446,486.844,134.565,446.025,92.206z" style="fill: rgb(255, 255, 255);"/>
            </g>
            </svg>
            <span class="skip_btn_text">10</span>
        </button>
        <button type="button" id="skip_forward_{{ forloop.counter }}" class="audio_btn position-relative">
            <svg class="audio_skip_forward" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="_x32_" x="0px" y="0px" viewBox="0 0 512 512" style="width:1.2rem;" xml:space="preserve">
            <style type="text/css">
                .st0{fill:#4B4B4B;}
            </style>
            <g>
                <path class="st0" d="M446.025,92.206c-40.762-42.394-97.487-69.642-160.383-72.182c-15.791-0.638-29.114,11.648-29.752,27.433   c-0.638,15.791,11.648,29.114,27.426,29.76c47.715,1.943,90.45,22.481,121.479,54.681c30.987,32.235,49.956,75.765,49.971,124.011   c-0.015,49.481-19.977,94.011-52.383,126.474c-32.462,32.413-76.999,52.368-126.472,52.382   c-49.474-0.015-94.025-19.97-126.474-52.382c-32.405-32.463-52.368-76.992-52.382-126.474c0-3.483,0.106-6.938,0.302-10.364   l34.091,16.827c3.702,1.824,8.002,1.852,11.35,0.086c3.362-1.788,5.349-5.137,5.264-8.896l-3.362-149.834   c-0.114-4.285-2.88-8.357-7.094-10.464c-4.242-2.071-9.166-1.809-12.613,0.738L4.008,182.45c-3.05,2.221-4.498,5.831-3.86,9.577   c0.61,3.759,3.249,7.143,6.966,8.974l35.722,17.629c-1.937,12.166-3.018,24.602-3.018,37.279   c-0.014,65.102,26.475,124.31,69.153,166.944C151.607,465.525,210.8,492.013,275.91,492   c65.095,0.014,124.302-26.475,166.937-69.146c42.678-42.635,69.167-101.842,69.154-166.944   C512.014,192.446,486.844,134.565,446.025,92.206z" style="fill: rgb(255, 255, 255);"/>
            </g>
            </svg>
            <span class="skip_btn_text">10</span>
        </button>
        <button type="button" id="vol_up_btn_{{ forloop.counter }}" class="audio_btn normal-line-height"><span>vol</span><br><span>+</span></button>
        <button type="button" id="vol_down_btn_{{ forloop.counter }}" class="audio_btn normal-line-height"><span>vol</span><br><span>-</span></button>
        <button type="button" id="muteBtn_{{ forloop.counter }}" class="audio_btn">
          <i class="fas fa-volume-up"></i>
        </button>
        <button type="button" id="rateUp_{{ forloop.counter }}" class="audio_btn normal-line-height">
          <span>rate</span><br><span>+</span>
        </button>
        <button type="button" id="rateDown_{{ forloop.counter }}" class="audio_btn normal-line-height">
          <span>rate</span><br><span>-</span>
        </button>
    </div>

    <div class="w-100">
        <div id="volText_{{ forloop.counter }}" class="text-center d-none"></div>
        <div id="audioRate_{{ forloop.counter }}" class="text-center d-none"></div>
    </div>

    <div class="container">
        <div class="row no-gutters">
            <div id="timeSlider_{{ forloop.counter }}" class="audio_time_bar_area col-8">
                <div id="audio_time_bar_{{ forloop.counter }}"></div>
            </div>
            <div class="col-4 text-center">
                <span id="minutes_{{ forloop.counter }}">00</span>:<span id="seconds_{{ forloop.counter }}">00</span>
                /<span id="audioMinutes_{{ forloop.counter }}"></span>:<span id="audioSeconds_{{ forloop.counter }}"></span>
            </div>
        </div>
    </div>
</div>
```

`css`

```

audio {
    width:100%;
}
.audio_area {
    display:flex;
    flex-wrap:wrap;
    padding:0.25rem 0.5rem;
}
.audio_btn {
    font-size:1rem;
    width:3rem;
    height:3rem;
    text-align:center;
    color:#fff;
    background: rgb(204,255,232);
    background-image: linear-gradient(90deg, rgba(247, 93, 139, 1), rgba(254, 220, 64, 1));
    border:none;
    border-radius:50%;
    margin:0.25rem;
}
.audio_btn:hover{
    background-image: linear-gradient(45deg, rgba(247, 93, 139, 1), rgba(254, 220, 64, 1));
    color:#7f7fff;
}
.audio_text {
    font-size:1.2rem;
}
.normal-line-height {line-height:normal;}
.audio_skip_forward {
    position: relative;
    margin-left:0.7rem;
    color: #fff;
    font-size:1.4rem;
    transform:rotate(180deg);
}
.audio_skip_backward {
    position: relative;
    margin-left:0.6rem;
    color: #fff;
    font-size:1.4rem;
    transform:scale(1,-1);
}
.skip_btn_text {
    position:absolute;
    font-size:0.8rem;
    transform:translate(-50%,-100%);
    color:#7f7fff;
}
.audio_wrapper {
    background: inherit;
    width:100%;
    padding:0.25rem 0;
    border:solid 1px #dbdbff;
    border-radius:0.25rem;
    height:auto;
}

.audio_time_bar_area {
    width:100%;
    height:1rem;
    background:#f2f2f2;
    cursor:pointer;
}
.audio_time_bar{
    height:1rem;
    background-image: linear-gradient(90deg, rgba(255, 146, 179, 1), rgba(255, 223, 74, 1));
    cursor:pointer;
}
```

`javascript`

`off("click")`と`e.stopPropagation();`は必要に応じて。

(追記：2023/1/11　javascriptの`window.addEventListener("load"~ });`を一部変更しました。)

(追記：2023/1/13　javascriptの不要な部分を削除。

停止ボタンの連打でintervalが無数に発行される可能性があるので、一部変更しました)
```

window.addEventListener("load" , function (){
    $(".startAudio").off("click").on("click", function(e){
        e.stopPropagation();
        const pk = $(this).attr("id").replace("start_button_","");
        setAudio(pk);
    });
});

function setAudio(pk){
    const audio = document.getElementById('audio_' + pk);
    audio.currentTime = 0;
    const wrapper       = document.getElementById('wrapper_' + pk);
    wrapper.classList.remove("d-none");
    const startButton   = document.getElementById('start_button_' + pk);
    const stopButton    = document.getElementById('stop_button_' + pk);
    const pauseButton   = document.getElementById('pause_button_' + pk);
    const skipForward   = document.getElementById('skip_forward_' + pk);
    const skipBackward  = document.getElementById('skip_backward_' + pk);
    const volUpButton   = document.getElementById('vol_up_btn_' + pk);
    const volDownButton = document.getElementById('vol_down_btn_' + pk);
    const volText       = document.getElementById('volText_' + pk);
    const muteBtn       = document.getElementById('muteBtn_' + pk);
    const rateUp        = document.getElementById('rateUp_' + pk);
    const rateDown      = document.getElementById('rateDown_' + pk);
    const audioMinutes  = document.getElementById("audioMinutes_" + pk);
    const audioSeconds  = document.getElementById("audioSeconds_" + pk);
    const audioRateTxt  = document.getElementById("audioRate_" + pk);

    var minutes         = document.getElementById("minutes_" + pk);
    var seconds         = document.getElementById("seconds_" + pk);
    var audioTime       = 0;
    var audioTimeBar    = document.getElementById("audio_time_bar_" + pk);
    var slider          = document.getElementById('timeSlider_' + pk);

    let paused     = false;
    let changeRate = false;
    let interval;

    function pad(val) { return val > 9 ? val : "0" + val; }
    audioMinutes.innerHTML = pad(Math.floor(audio.duration / 60));
    audioSeconds.innerHTML = pad(Math.floor(audio.duration % 60));

    startButton.classList.add("d-none");

    if( audio.volume === 0){
        muteBtn.innerHTML = '<i class="fas fa-volume-mute"></i>';
    }

    audio.onended = () => {
        stopAudio();
        paused = true;
        //audioCounter();　不要なので削除。
    }
    stopButton.onclick = (e) => {
        e.stopPropagation();
        audio.pause();
        stopAudio();
        paused = true;
        //audioCounter();　不要なので削除。
    }
    pauseButton.onclick = (e) => {
        e.stopPropagation();
        if ( paused ) {
            audio.play();
            pauseButton.innerHTML = '<i class="fas fa-pause"></i>';
            paused = false;
            audioCounter();
        } else {
            audio.pause();
            pauseButton.innerHTML = '<i class="fas fa-play"></i>';
            paused = true;
            
            //追記：ボタン連打でintervalが無数に発生する可能性があるので、ここで停止。
            clearInterval(interval);
            delete interval;
        }
    }
    skipForward.onclick = (e) => {
        e.stopPropagation();
        audioTime += 10;
        if (audioTime > audio.duration){
            audioTime = audio.duration;
        }
        audio.currentTime = audioTime;
        setCounter();
    }
    skipBackward.onclick = (e) => {
        e.stopPropagation();
        audioTime -= 10;
        if (audioTime < 0){
            audioTime = 0;
        }
        audio.currentTime = audioTime;
        setCounter();
    }
    volUpButton.onclick = (e) => {
        e.stopPropagation();
        if(audio.volume <= 0.9){
        audio.volume += 0.1;
        volText.textContent = "vol : " + Math.round((audio.volume * 10)) / 10;
        audioText(volText);
        }else{;}
    }
    volDownButton.onclick = (e) => {
        e.stopPropagation();
        if(audio.volume >= 0.1){
            audio.volume -= 0.1;
            volText.textContent = "vol : " + Math.round((audio.volume * 10)) / 10;
            audioText(volText);
        }else{
            audio.volume = 0;
            volText.textContent = "vol : " + 0;
            audioText(volText);
        }
    }
    muteBtn.onclick = (e) => {
        e.stopPropagation();
        if( audio.muted ){
            muteBtn.innerHTML = '<i class="fas fa-volume-up"></i>';
            audio.muted = false;
            volText.textContent = "vol : " + Math.round((audio.volume * 10)) / 10;
            audioText(volText);
        } else {
            muteBtn.innerHTML = '<i class="fas fa-volume-mute"></i>';
            audio.muted = true;
            volText.textContent = "vol : " + 0;
            audioText(volText);
        }
    }
    rateUp.onclick = (e) => {
        e.stopPropagation();
        audio.playbackRate += 0.1;
        audioRateTxt.textContent = Math.round((audio.playbackRate * 10)) / 10 + "×";
        audioText(audioRateTxt);
    }
    rateDown.onclick = (e) => {
        e.stopPropagation();
        if(audio.playbackRate > 0.1){
            audio.playbackRate -= 0.1;
        }
        audioRateTxt.textContent = Math.round((audio.playbackRate * 10)) / 10  + "×";
        audioText(audioRateTxt);
    }
    slider.onclick = (e) => {
        e.stopPropagation();
//        var click_x = e.pageX ;
//        var client_rect = slider.getBoundingClientRect();
//        var position_x = client_rect.left + window.pageXOffset;
//        var x = click_x - position_x ;
//        audioTime = Math.round( audio.duration * (x / client_rect.width) );
        audioTime = Math.round( audio.duration * e.offsetX /  slider.clientWidth)
        audio.currentTime = audioTime;
        setCounter();
    }

    function stopAudio() {
        startButton.classList.remove("d-none");
        minutes.innerHTML = "00";
        seconds.innerHTML = "00";
        wrapper.classList.add("d-none");
        if( paused ){
            pauseButton.innerHTML = '<i class="fas fa-pause"></i>';
        }
    }
    function setAudioTimeBar () {
        audio.ontimeupdate = () =>{
            var Width = (audio.currentTime / audio.duration * 100) + '%';
            audioTimeBar.style.width = Width;
            audioTimeBar.classList.add("audio_time_bar");
        }
    }
    function audioCounter(){
        let interval = setInterval( function(){
            if (!paused){
                audioTime += audio.playbackRate;
                setCounter();
            }else{
                clearInterval(interval);
                delete interval;
            }
        }, 1000);
    }
    function setCounter(){
        minutes.innerHTML = pad(Math.floor(audioTime / 60));
        seconds.innerHTML = pad(Math.floor(audioTime % 60));
    }
    function audioText(elem){
        elem.classList.remove("d-none");
        var showText = setTimeout( function(){
            elem.classList.add("d-none");
            clearTimeout( showText );
        }, 700);
    }
    audioCounter();
    audio.play();
    setAudioTimeBar();
}

```

##### 参考記事

[〇Math.floor()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)

与えられた数値以下の最大の整数を返す。

[〇Math.round()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/round)

引数として与えた数を四捨五入して、もっとも近似の整数を返す。

[〇parseInt()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

文字列の引数を解析し、指定された基数 (数学的記数法の底) の整数値を返す。

parseIntをMath.floor()の代用として使用してはいけない。

[〇setTimeout,setInterval](https://techplay.jp/column/548)

[〇JavaScript カウントアップタイマー](https://www.delftstack.com/ja/howto/javascript/javascript-count-up-timer/)

[〇addeventListener　要素数に応じてイベントが重複する](https://teratail.com/questions/329788)

##### お薦め

{{<x user="Rapt_plusalpha" id="1604424547159011328">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1604420441031839744">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1604098755124682752">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1604086070009659393">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1604058845521006592">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1604057319188615168">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1603714497608183808">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1603341600389677056">}}
&nbsp;