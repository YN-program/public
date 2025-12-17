---
title: "ffmpegで動画に縁取りを付けてみた。"
description: ""
date: "2022-07-26T08:49:56+09:00"
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
[前回記事](https://progress-advance.com/programming/ffmpeg-add-audio2video/)で、ffmpeg を用いて動画にワイプ動画を重ねましたが、ワイプ動画に縁取りが欲しいと思いましたので、トライしてみました。

<!--more-->

動画にランダムな色で 2px の縁取りを付けるコードです。

```
ffmpeg -i input.mp4 -vf "pad=iw+4:ih+4:2:2:random@1" output.mp4
```

`random@1`を`white`,`color=blue`のように色を指定することもできます。

`iw+4`は、video width + 4px、`ih+4`は、video height + 4px、`pad=iw+4:ih+4:2:2`の「2」で、上下左右に2pxの縁取りを追加します。

{{<file>}}
<video width=100% controls>
    <source src="/img/video/watermark.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>
{{</file>}}
&nbsp;

##### 参考
[FFmpeg Filters Documentation](https://ffmpeg.org/ffmpeg-filters.html#pad-1)

[ffmpeg set padding with different top and bottom bar background colors](https://stackoverflow.com/questions/56815993/ffmpeg-set-padding-with-different-top-and-bottom-bar-background-colors)
&nbsp;

[Add unique color watermark per frame with FFMpeg](https://stackoverflow.com/questions/52998126/add-unique-color-watermark-per-frame-with-ffmpeg)

[FFMPEG - Padding bottom of image](https://stackoverflow.com/questions/66615701/ffmpeg-padding-bottom-of-image)

{{<youtube uLMCrotl1CU>}}

&nbsp;

##### お薦め
[【ガーシー大誤算】YouTubeで切り抜き部隊が続々と一発BAN　苦肉の策で動画配信サロンを計画するも、中国企業アリババのサーバーを利用、かつ月10万円の会費に批判殺到](https://rapt-plusalpha.com/49492/)

[【じげもんの常識をブッ壊せ!!】Vol.17 – 群馬人脈の最重要人物・笹川良一　中国共産党と結託し、日本にユダヤ人国家の建国を目論む](https://rapt-plusalpha.com/49442/)

[「堀江貴文」がコロナワクチン接種を拒んだとの理由からアーティストの「CEO（セオ）」と絶縁 「今からでも言うことを聞いてワクチンを打てばいい」と接種を強要](https://rapt-plusalpha.com/49414/)
&nbsp;

{{<youtube gNaZBg3jl9g>}}
