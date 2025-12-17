---
title: "【ffmpeg】音声付き動画の合成"
description: ""
date: "2022-07-29T08:40:37+09:00"
thumbnail: 
  src: "img/thumb/hiyo2.png"
  visibility:
    - list
categories:
  - "programming"
tags:
  - "ffmpeg"
  
menu:
main:
  name: プログラミング
  weight: 2
---

前々回の記事で、2つの動画を合成する際に、一度に動画と音声を合成することが出来なかったので、再チャレンジしてみました。

<!--more-->

[【ffmpeg】画像と音源から動画作成・動画と動画の合成・動画と音声の合成](https://progress-advance.com/programming/ffmpeg-add-audio2video/)

前回の状態。

![pre](/img/2.png)

今回の目標。

![post](/img/1.png)


コマンド

```
ffmpeg -i hiyo.mp4 -i resized.mp4 -filter_complex "overlay=x=(W-w)/10:y=(H-h)/10;[0:a][1:a] amix=inputs=2:duration=longest [audio_out]" -map 0:v -map "[audio_out]" -y final.mp4
```

`hiyo.mp4`

{{<file>}}
<video width=100% controls>
    <source src="/img/video/hiyo.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>
{{</file>}}
&nbsp;

`resized.mp4`

{{<file>}}
<video width=100% controls>
    <source src="/img/video/resized.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>
{{</file>}}
&nbsp;

合成結果 `final.mp4`

{{<file>}}
<video width=100% controls>
    <source src="/img/video/mix.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>
{{</file>}}
&nbsp;

今回のコマンドは、2つの音声を1つの動画に合成するコマンドと、動画を重ね合わせるコマンドを繋げただけのものになりますが、めでたく目標達成出来ました。
&nbsp;

##### 参考記事
[映像の上に映像をのせる](https://nico-lab.net/overlay_with_ffmpeg/)

[how to add audio to video with FFMPEG](https://json2video.com/how-to/ffmpeg-course/ffmpeg-add-audio-to-video.html)

##### お薦め

[Facebookを運営する「Meta」株価暴落や大幅減収が相まり、コロナやワクチン関連情報の削除を取りやめ検討へ 従業員のリストラが始まる噂も](https://rapt-plusalpha.com/49671/)

[【さらなる信用失墜】Twitterユーザー540万人分の個人情報が「ハッキングフォーラム」で約4000万円で販売されていたことが発覚](https://rapt-plusalpha.com/49577/)

[中国共産党のスパイ「ひろゆき」が「ワクチンが原因で亡くなった人はいない」との虚偽情報をYouTubeで拡散　「堀江貴文」もコロナワクチンをゴリ押ししすぎて国民から完全に嫌われる](https://rapt-plusalpha.com/49695/)

[神様を第一に愛することで、家族との問題をすべて解決してくださり、家族を幸福にしてくださった神様の偉大な愛（十二弟子・KAWATAさんの証）](https://rapt-plusalpha.com/49596/)

[【一帯一路の拠点】大阪府が「医療非常事態宣言」 65歳以上の高齢者に外出自粛要請 中国共産党による乗っ取り工作が加速](https://rapt-plusalpha.com/49751/)