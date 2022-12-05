---
title: "Audio-Css"
description: ""
date: "2022-11-21T09:04:59+09:00"
thumbnail: 
  src: "img/thumb/"
  visibility:
    -list
categories:
  - "programming"
tags:
  - "javascript"
menu:
  main:
    name: プログラミング
    weight: 2
---
{{<file>}}

<audio class="audioPlayer" src="/img/audio/hiyo.mp3" type="audio/mp3" controls>
</audio>

<style>

audio {
  background: rgb(253,208,255);
  height:4rem;
  border-radius: 0.5rem;
  padding:0.25rem 0.5rem;
}

audio::-webkit-media-controls-panel {
  background: #fff;
}

</style>
{{</file>}}

```

<audio src="/img/audio/***.mp3" type="audio/mp3" controls></audio>

<style>

audio {
  background: rgb(253,208,255);
  height:4rem;
  border-radius: 0.5rem;
  padding:0.25rem 0.5rem;
}

audio::-webkit-media-controls-panel {
  background: #fff;
}

</style>
```

{{<file>}}

<style>

.audio-area {
  display:flex;
  flex:nowrap;
  padding:0.25rem 0.5rem;
  background: #f7f7f7;
}

.audio_btn {
  font-size:1.5rem;
}
.muteBtn { display:none;}
.volBtn { display:block;}
.volume_area {
  display:flex;
  flex:nowrap;
  position:relative;
}
.volume {
  width:5rem;
  margin:0 0.5rem;
}
</style>

<audio id="my-audio" src="/img/audio/hiyo.mp3" type="audio/mp3" controls></audio>

<div class=audio-area>
  <button id="play-button" class="audio_btn">&#9205;</button>
  <button id="pause-button" class="audio_btn">&#9208;</button>
  <button id="skip-backward" class="audio_btn">&#171;</button>
  <button id="skip-forward" class="audio_btn">&#187;</button>
  <button id="vol-up-btn" class="audio_btn">+</button>
  <button id="vol-down-btn" class="audio_btn">-</button>
  <button id="muteTrue" class="muteBtn">&#128263;</button>
  <button id="muteFalse" class="volBtn">&#128266;</button>
  <div class="time_line_area" >
    <input type="range" id="time" name="time" class="time">
    <label for="time" id="time_label" class="audio_btn"></label>
  </div>
  <div class="volume_area" >
    <input type="range" id="volume" name="volume" class="volume" min="0" max="1" step="0.1" value="1">
    <label for="volume" id="vol" class="audio_btn"></label>
  </div>
</div>



<script>
var myAudio = document.getElementById('my-audio');
var playButton = document.getElementById('play-button');
var pauseButton = document.getElementById('pause-button');
var skipForward = document.getElementById('skip-forward');
var skipBackward = document.getElementById('skip-backward');
var volUpButton = document.getElementById('vol-up-btn');
var volDownButton = document.getElementById('vol-down-btn');
var vol = document.getElementById('vol');
var muteBtn = document.getElementById('muteTrue');
var muteFalse = document.getElementById('muteFalse');
var volume = document.getElementById("volume");
var time_line = document.getElementById("time");
var time_label = document.getElementById("time_label");

playButton.addEventListener("click", function(){
    myAudio.play();
});
pauseButton.addEventListener("click", function(){
    myAudio.pause();
});
skipForward.addEventListener("click", function(){
    myAudio.currentTime += 1;
});
skipBackward.addEventListener("click", function(){
    myAudio.currentTime -= 1;
});
volUpButton.addEventListener("click", function(){
  if(myAudio.volume < 1){
    myAudio.volume += 0.1;
    vol.textContent = Math.round((myAudio.volume * 10)) / 10;
  }
});
volDownButton.addEventListener("click", function(){
  if(myAudio.volume > 0.09){
    myAudio.volume -= 0.1;
    vol.textContent = Math.round((myAudio.volume * 10)) / 10;
  }
});
muteFalse.addEventListener("click", function(){
  muteFalse.classList.remove('volBtn');
  muteFalse.classList.add('muteBtn');
  muteBtn.classList.remove('muteBtn');
  muteBtn.classList.add('volBtn');
  myAudio.volume = 0;
  vol.textContent = 0;
});
muteBtn.addEventListener("click", function(){
  muteFalse.classList.remove('muteBtn');
  muteFalse.classList.add('volBtn');
  muteBtn.classList.remove('volBtn');
  muteBtn.classList.add('muteBtn');
  myAudio.volume = 0.1;
  vol.textContent = 0.1;
});
volume.addEventListener("change", function(){
  myAudio.volume = volume.value;
  vol.textContent = volume.value;
});


</script>
{{</file>}}
&nbsp;

##### 参考記事

[JavaScript 数値丸め 切り捨て、切り上げ、四捨五入（floor、ceil、round）](https://johobase.com/javascript-floor-ceil-round/#i-5)

[Coding a Custom HTML Audio Player (Part 1)](https://pointclearmedia.com/2020/08/31/coding-a-custom-html-audio-player/)

[CSS Styling the Audio Element
](https://pointclearmedia.com/2020/08/27/css-styling-the-audio-element/)