---
title: "MoviePyでワイプ画面の追加"
description: ""
date: "2022-06-26T09:32:32+09:00"
thumbnail: "img/moviepy/wipe.png"
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

無料の動画編集ソフトでも動画にワイプ画面を追加できますが、MoviePyでもできるということで試してみました。

<!--more-->

コードは[こちら](https://python-climbing.com/python_miviepy_wipe/)を参考にしました。

```
from moviepy.editor import *

file_path1 = ""
file_path2 = ""
save_path = ""

video1 = VideoFileClip(file_path1)
video2 = VideoFileClip(file_path2)
w1,h1 = moviesize = video1.size
w2,h2 = moviesize2 = video2.size

# 動画のサイズにより、縮小率を変更

if w1 > h1 and w2 > h2:
    w = w1 / 4
    h = w * h2 / w2

elif w1 < h1 and w2 > h2:
    w = w1 / 2
    h = w  * h2 / w2 

elif w1 > h1 and w2 < h2:
    h = h1 / 2
    w = h *  w2 / h2

else:
    h = h1 / 4
    w = h *  w2 / h2

# 短いほうの動画に再生時間を合わせるために、時間を取得。

duration1 = video1.duration
duration2 = video2.duration
if duration2 >= duration1:
        d = int(duration1)
else:
        d = int(duration2)

wipe = (VideoFileClip(file_path2).       # ワイプ動画をロード
        resize((w, h)).                  # ワイプサイズを設定
        margin( 2,color=(255,255,255)).  # マージンを白にして枠をつくる
        margin( bottom=20, right=20, opacity=0).    # 位置の微調整。右側と下側に余白をつくる
        set_pos(('right','bottom')) )               # セットポジション

final_clip = CompositeVideoClip([video1, wipe])
final_clip.subclip(0,d).write_videofile(save_path, fps=30) # 0～d秒間を書き出し

```

その他、画面の反転、フェードイン、動画の結合、逆再生などもできるようなので、もう少し研究してみようと思います。

https://moviepy-tburrows13.readthedocs.io/en/latest/getting_started/compositing.html

##### お薦め動画

{{<youtube mJSBdQ0oBPg>}}
&nbsp;

{{<youtube 0HQT34jbkls>}}
&nbsp;
{{<youtube 6W9hPx3hH5s>}}
&nbsp;
{{<youtube dZusTa6srSw>}}
&nbsp;
{{<youtube PQv2PlyjhbE>}}