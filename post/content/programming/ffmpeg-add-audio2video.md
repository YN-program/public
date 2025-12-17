---
title: "【ffmpeg】画像と音源から動画作成・動画と動画の合成・動画と音声の合成"
description: ""
date: "2022-07-24T10:41:09+09:00"
thumbnail: 
  src: "img/thumb/hiyo.jpg"
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

1枚の画像 hiyo.jpg とオーディオファイル hiyo.mp3 から動画 hiyo.mp4 を作成（画像は[写真のフリー素材サイト](https://www.photo-ac.com/)から、音源は[効果音ラボ](https://soundeffect-lab.info/sound/animal/)から）。

<!--more-->

「hiyo.jpg」
&nbsp;

![hiyo](/img/thumb/hiyo.jpg)
&nbsp;

「hiyo.mp3」

{{<file>}}
    <audio
        controls
        src="/img/audio/hiyo.mp3">
            Your browser does not support the
    </audio>
{{</file>}}
&nbsp;

「hiyo2.mp3」

{{<file>}}
    <audio
        controls
        src="/img/audio/hiyo3.mp3">
            Your browser does not support the
    </audio>
{{</file>}}
&nbsp;

hiyo.jpg と hiyo.mp3 から動画を作成。

```
ffmpeg -loop 1 -r 30000/1001 -i hiyo.jpg -i hiyo.mp3 -vcodec libx264 -acodec aac -strict experimental -ab 320k -ac 2 -ar 48000 -pix_fmt yuv420p -shortest -s 1280*720 hiyo.mp4
```

ワイプ動画として重ね合わせる動画を作成。音源は hiyo2.mp3を使用。

```
ffmpeg -loop 1 -r 30000/1001 -i hiyo.jpg -i hiyo2.mp3 -vcodec libx264 -acodec aac -strict experimental -ab 320k -ac 2 -ar 48000 -pix_fmt yuv420p -shortest -s 1280*720 hiyo2.mp4
```

[&nbsp; FFmpegでの音声ファイルと静止画1枚からの動画の作成方法](https://dev.classmethod.jp/articles/ffmpeg-create-movie-by-audio/)

[【タイムラプス】FFmpegで静止画から動画を作成する方法【CinemaDNG】](https://zapanet.info/blog/item/2653)

##### -loop 1

1枚の画像を動画に。

##### -r 30000/1001

フレームレートを指定（29.97fps）。「-r 30」という書き方もある。

##### -i hiyo.png -i hiyo.mp3

入力ファイルを指定。

##### -vcodec libx264

動画フォーマットをH.264に指定。

##### -acodec aac -strict experimental -ab 320k -ac 2 -ar 48000

音声フォーマットを指定。AACで320kbps、2チャンネル、サンプリングレート48kHz。

##### -pix_fmt yuv420p

出力動画の画素フォーマットを指定。

##### -shortest

動画の長さを入力ソースが最短のものに合わせる。

##### -s 1280*720

動画サイズを1280*720に。

##### hiyo.mp4

出力ファイルの名称を最後に記載。

***

ビットレートを指定する場合は、動画サイズの前に「-b:v 12000k」を追記することで、12Mbps になる。

***

ワイプ動画用に hiyo2.mp4 のサイズ変更。
```
ffmpeg -i hiyo2.mp4 -s 320x180 resized.mp4
```

hiyo.mp4 に resized.mp4 を重ね合わせ。
```
ffmpeg -i hiyo.mp4 -i resized.mp4 -filter_complex "overlay=x=(W-w)/10:y=(H-h)/10" -c:a copy add_resized.mp4
```

`overlay`フィルタについては、以下の記事を参考にしました。

[映像の上に映像をのせる OVERLAY](https://nico-lab.net/overlay_with_ffmpeg/)

合成された動画には、hiyo.mp4 の音のみで、resized.mp4 の音が入っていなかったので、再度 hiyo2.mp3 を追加。

```
ffmpeg -i add_resized.mp4 -i hiyo2.mp3 -c:v copy -filter_complex "[0:a][1:a] amix=inputs=2:duration=longest [audio_out]" -map 0:v -map "[audio_out]" -y final.mp4
```

動画とオーディオファイルの合成は、以下の記事が分かりやすかったです。

[How to add audio to video with FFMPEG](https://json2video.com/how-to/ffmpeg-course/ffmpeg-add-audio-to-video.html)


結果。

{{<file>}}
<video width=100% controls>
    <source src="/img/video/final.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>
{{</file>}}
&nbsp;

動画の合成時にワイプ動画の音が合成されなかったので、以下のようにワイプ用の動画は音源なしで作成し、最後に音を追加すると無駄がなかった。コード内の `-t 3` を指定することで3秒の動画になる。

```
ffmpeg -loop 1 -i hiyo.jpg -vcodec libx264 -pix_fmt yuv420p -t 3 -r 30 -s 1280*720 out.mp4
```
&nbsp;

##### お薦め記事

[米Twitter社4～6月期決算で2億7000万ドル（約370億円）の赤字  イーロン・マスクの買収撤回が収益を圧迫したと批判](https://rapt-plusalpha.com/49317/)

[インドネシアの格安航空会社のパイロットがコロナワクチン接種後、乗客171人を乗せた旅客機のフライト中に心臓発作を起こして死亡](https://rapt-plusalpha.com/49284/)

[【それでも検査を受けますか？】「PCR検査」で使用する綿棒は、不衛生な環境でノーマスクかつ素手の作業員がパック詰めしている](https://rapt-plusalpha.com/49265/)

[【イギリス政府がコロナワクチンの危険性を認める】副反応によって死亡した人の遺族らに12万ポンド（約1900万円）の補償金　 一方の日本では5回目のワクチン接種の準備](https://rapt-plusalpha.com/49219/)

[【NHK党の立花孝志と統一教会は密接に繋がっている】怖い物知らずの立花孝志「統一教会は怖いので、これからもあまり言及しないと思います」と尻込み](https://rapt-plusalpha.com/49155/)

[RAPTさんの周りでは、常に神様が臨まれ、祝福と恵みが満ち溢れている!!（十二弟子・ミナさんの証）](https://rapt-plusalpha.com/49140/)

##### お薦め動画

{{<youtube x1l7JZyMzWI>}}