---
title: "ウェブカメラで撮影した動画(webmファイル)のdurationがInfinityになる問題"
description: ""
date: "2022-07-05T22:42:34+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "MediaRecorder"
  - "Webm"
  - "MoviePy"
  - "python"
menu:
main:
  name: プログラミング
  weight: 2
---
先日、[ウェブカメラで撮影した動画を、直接サーバーに送る](https://progress-advance.com/programming/webcamvideotoserver/)記事を記載しました。

動画をアップロードするまでは良かったのですが、MediaRecorder で録画した webm の duration が Infinity になり、MoviePy で読み込むことができず、動画を編集することができませんでした。

<!--more-->

対策としては、「fix-webm-duration」を使用するか、「ffmpeg」を使えば良さそうです。

[MediaRecorder で録画した WebM の duration が Infinity になる問題](https://qiita.com/legokichi/items/14a8d39dbaa90c2a5ccb)

[GitHug fix-webm-duration](https://github.com/yusitnikov/fix-webm-duration)

[How to determine webm duration using ffprobe](https://stackoverflow.com/questions/34118013/how-to-determine-webm-duration-using-ffprobe)

というわけで、今回は ffmpeg をパソコンにインストールして、ffmpeg を python で使えるように ffmpeg-python をインストールしました。

で、webm ファイルを mp4 に変換してみたところ、無事に MoviePy で読み込むことができました。

##### ffmpegでファイルを変換する方法(python)

```
import subprocess

subprocess.call('ffmpeg -i input.webm output.mp4', shell=True)
```
または、
```
import os
import subprocess as sp

input_file = ''
file_name = os.path.splitext(os.path.basename(input_file))[0]

cmd_list = ['ffmpeg', '-i', input_file, file_name + '.mp4']
cmd = ' '.join(cmd_list)
sp.call(cmd, shell=True)
```


[ffmpeg-pythonで動画編集する](https://qiita.com/studio_haneya/items/a2a6664c155cfa90ddcf)

[ファイル変換はffmpegが本当に便利！Pythonでも使える](https://watlab-blog.com/2021/05/08/ffmpeg-python/)

[Python MOVをmp4へ変換する。ffmpegをsubprocess.callで呼び出す](https://hk29.hatenablog.jp/entry/2021/09/18/235829)


なお、`shell=True`はセキュリティ上のリスクになるので、注意が必要とのこと。

[subprocessの使い方(Python3.6)](https://qiita.com/caprest/items/0245a16825789b0263ad)

[pythonでOSコマンドを実行する(subprocess.run利用)](https://qiita.com/cloud-solution/items/b7b05ce0f55dbfbeb36a)

以上です。

##### お薦め

選挙前に、政治家たち、支配者たちの闇が次々と暴かれています。

ジョン・レノンは生前、「世界は狂人によって支配されている」と語りましたが、彼らのキチガイぶりは私たちの想像を遥かに超えています。

本当に沢山の方が、RAPTブログ、RAPT理論+αをお読みになり、彼らの正体を知り、彼らの洗脳から抜け出し、真に甲斐のある人生を生きられますように願います。

[ロックバンド「ビートルズ」は中国共産党に取り込まれ、悪魔崇拝思想や麻薬を広める“洗脳装置”となった](https://rapt-plusalpha.com/46556/)

[参政党の「神谷宗幣」は、“森友学園事件”に核心的に関与していたことが発覚](https://rapt-plusalpha.com/46631/)

[中国海軍が尖閣水域に進入するも、日本政府は「懸念」を表明し抗議するのみ 記者会見した木原誠二官房副長官は、三木谷ルームのパーティに参加していたことが発覚](https://rapt-plusalpha.com/46625/)

[【NHK党・立花孝志】人口削減や大量殺戮を肯定「アホみたいに子供を産む民族は虐殺しろ」「馬鹿に一票入れてもらう方法を考えるのが本当に賢い人」](https://rapt-plusalpha.com/46614/)

[【楽天・三木谷会長の性スキャンダル問題】“楽天潰し”は習近平派による江沢民派への攻撃である可能性大 日本でも激化する中共の派閥争い](https://rapt-plusalpha.com/46522/)

[【じげもんの常識をブッ壊せ!!】Vol.14 – 恐竜と人類は共存していた!! 現代科学の全ての学説が神を否定するために存在している](https://rapt-plusalpha.com/46524/)

{{<youtube 4zYsDE5XH6s>}}