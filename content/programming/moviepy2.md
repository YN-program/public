---
title: "MoviePyで動画の切出し/結合・リサイズ・スローモーション"
description: ""
date: "2022-07-12T11:41:10+09:00"
thumbnail: "img/thumb/abeShot.png"
categories:
  - "programming"
tags:
  - "python"
  - "MoviePy"
menu:
main:
  name: プログラミング
  weight: 2
---
MoviePy で動画が編集できるということで、少し触ってみた。
<!--more-->

```
from moviepy.editor import *

file_path = "file/to/path.mp4" # 動画のパス


video = VideoFileClip(file_path) # 動画をロード
w,h = moviesize = video.size     # 動画サイズを取得
print(w,h)

## speedxの関数は公式ページから引用
## https://zulko.github.io/moviepy/_modules/moviepy/video/fx/speedx.html#speedx

def speedx(clip, factor=None, final_duration=None):

  if final_duration:
    factor = 1.0 * clip.duration / final_duration

  newclip = clip.fl_time(lambda t: factor * t, apply_to=['mask', 'audio'])

  if clip.duration is not None:
    newclip = newclip.set_duration(1.0 * clip.duration / factor)

  return newclip

# 動画の切り出し（10~20秒、16~18秒、18~25秒を切り出した）
clip1 = video.subclip(10, 20)
clip2 = video.subclip(16, 18)
clip3 = video.subclip(18, 25)

# clip2 から動画を切り出し(開始地点(x1,x2)、横幅 x2、高さ y2)
clip4 = clip2.crop(x1=230, y1=108, x2=454, y2=234)

# 切り出した動画を、元の動画と同じサイズ(w = 640, h = 360)にリサイズ
# width=640 とすることで、アスペクト比を保ったまま width 640pxの動画が書き出される
clip5 = clip4.fx(vfx.resize, width=640)

# clip5 のスローモーション
clip6 = speedx(clip5, 0.2, 10)

# clip5 を更に切り出して、スローモーション
clip7 = clip5.subclip(0.5, 2)
clip8 = speedx(clip7, 0.2, 10)

# 動画の結合、書き出し
final_clip = concatenate_videoclips([clip1, clip5, clip6, clip8, clip3])
final_clip.write_videofile("final.mp4")
```

出来た動画（安倍晋三銃撃動画）は、ツイッターに投稿。

{{<x user="GLnoIrjwa7omiK7" id="1546683779322097664">}}

&nbsp;

テキストの挿入は、MoviPy の場合、ImageMagick のインストール及び PATH の設定が必要だったので、今回はパスして手元にある動画編集ソフトを使用しました(/ω＼)

まだまだ色々なことが出来そうなので、機会があれば触ってみようと思います。

##### 参考記事

[MoviePyを使ってPythonで動画編集をする](https://qiita.com/hosokawaR/items/ae349122be575b5e546c)

[【Python】動画へのテロップ挿入【MoviePy】](https://plantprogramer.com/python_moviepy/)

[Python/MoviePyで動画編集【カット・トリミング・テキスト挿入】をする！](https://gadgelaun.com/?p=25112)

##### お薦め記事

[【資金難か？】イーロン・マスクが米Twitter社の買収撤回を表明 はしごを外されたTwitter社は提訴する構え](https://rapt-plusalpha.com/48169/)

[【ハニートラップの重要拠点】楽天・三木谷会長と岸田首相の側近・木原誠二とパソナが、結託して中共のためにスパイ活動を行っている可能性大](https://rapt-plusalpha.com/47999/)

[【じげもんの常識をブッ壊せ!!】Vol.15 – かつての日本には二つの国が存在していた!!　群馬人脈のルーツは古代東日本の「日本王国」](https://rapt-plusalpha.com/48144/)

[【第30回】ミナのラジオ – X JAPANの闇を暴く　Toshiの洗脳も、hideとTAIJIの死も、中国共産党の犯行だった‼︎ – ゲスト・RAPTさん&エリカさん](https://rapt-plusalpha.com/45160/)

[【第29回】ミナのラジオ – 地下鉄サリン事件を計画・実行したのも実は中国共産党だった‼︎ – ゲスト・KAWATAさん](https://rapt-plusalpha.com/44667/)