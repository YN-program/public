---
title: "【Python】ffmpeg での動画の重ね合わせ、動画とオーディオファイルの合成"
description: ""
date: "2022-08-05T14:19:47+09:00"
thumbnail: ""

categories:
  - "programming"
tags:
  - "ffmpeg"
  - "python"
  
menu:
main:
  name: プログラミング
  weight: 2
---
以前 ffmpeg で動画・音声を合成する記事を記載しました。

[【ffmpeg】音声付き動画の合成](https://progress-advance.com/programming/ffmpeg-watermark-video-audio-mix/)

今回は python の環境で ffmpeg を使用して動画の重ね合わせ、動画と音声の合成をやってみました。

<!--more-->

音声付き動画とオーディオファイルの合成。

```
import subprocess

# javascriptで動画とオーディオのボリューム、音声の合成スタート時間を取得し、POSTで送信後。
# 動画とオーディオファイルは、バリデーションして保存済み。

video_volume = request.POST["parent_volume"]
audio_volume = request.POST["audio_volume"]
delay_time   = request.POST["audio_delay"]

cmd_list1 = ['ffmpeg', '-i', video_path, '-ss', delay_time ,'-accurate_seek', '-i', audio_path, '-c:v', 'copy', '-filter_complex',
              '[0:a] volume='+ video_volume + '[m];[1:a] volume=' + audio_volume + '[m2];[m][m2] amix=inputs=2:duration=longest [audio_out]',
              '-map', '0:v', '-map', '[audio_out]', '-y', 'output.mp4']
subprocess.call(cmd_list1, shell=False)

output = 'output.mp4'

# ffmpeg-normalize で音量調節

cmd_list2 = ['ffmpeg-normalize', output, '-o', 'final.mp4', '-c:a','aac','-b:a','192k']
subprocess.call(cmd_list2, shell=False)
```

2つの音声付き動画ファイルの重ね合わせ。

```
import subprocess

# javascriptで動画のボリューム、ワイプ動画の合成スタート時間、ワイプ動画の位置、サイズを取得し、POSTで送信後。
# 動画ファイルは、バリデーションして保存済み。

wipe_video_path, video_path = # それぞれの動画のpath
video_volume = request.POST["video_volume"]
wipe_volume  = request.POST["wipe_volume"]
delay_time = request.POST["video_delay"]

wipe_x, wipe_y = # ワイプ動画の左上のx,y座標。
w,h = # ワイプ動画のwidth, height。背景の動画のサイズに合わせて変更。


cmd_list1 = ['ffmpeg', '-i', wipe_video_path, '-vf', 'pad=iw+20:ih+20:10:10:random@1','-s', w + '*' + h, 'resized.mp4' ]
subprocess.call(cmd_list1, shell=False)

resized_video = 'resized.mp4'

cmd_list2 = ['ffmpeg','-i', video_path, '-ss', delay_time ,'-accurate_seek', '-i', resized_video,
            '-filter_complex','overlay=x=' + wipe_x + ':y=' + wipe_y + ';[0:a] volume=' + video_volume + '[m];[1:a] volume=' + wipe_volume + '[m2];[m][m2] amix=inputs=2:duration=longest [audio_out]',
            '-map', '0:v', '-map','[audio_out]', '-y', 'mixed.mp4']
subprocess.call(cmd_list2, shell=False)

mixed_video = 'mixed.mp4'

cmd_list3 = ['ffmpeg-normalize', mixed_video, '-o', 'final.mp4', '-c:a','aac','-b:a','192k']
subprocess.call(cmd_list3, shell=False)

```

以上になります。

##### 参考記事
[ffmpeg-normalize でMP3ファイルの音量を平均化](https://opty-life.com/study/program/ffmpeg-normalize/)

[ffmpegを使用してオーディオを正規化するにはどうすればよいですか？](https://qastack.jp/superuser/323119/how-can-i-normalize-audio-using-ffmpeg)

##### お薦め記事

[【ロシア】ウクライナ騒乱を巡る「偽情報」や未成年者に有害な情報を含むコンテンツの削除要請に応じなかったとして、米Googleに罰金500億円を科す](https://rapt-plusalpha.com/50214/)

[【カナダ・アルバータ州】コロナワクチン接種を実施した結果、2021年の死因ランキング1位が「死因不明」に　](https://rapt-plusalpha.com/50243/)

[【北海道】札幌禎心会病院が「mRNAワクチンそのものに欠陥があることが判明した」としてコロナワクチン接種中止を宣言 ファイザーの隠蔽する副反応のデータにも言及](https://rapt-plusalpha.com/50227/)

[RAPTさんが神様と完全に一体になられていることをはっきりと目の当たりにした不思議な体験（十二弟子・KAWATAさんの証）](https://rapt-plusalpha.com/50183/)

[【安倍晋三銃撃事件は茶番】「奈良県立医科大学」が安倍晋三元首相の死亡を証明する行政文書の開示決定を延長](https://rapt-plusalpha.com/50162/)

[「南京大虐殺」は、中国人による日本人虐殺「通州事件」を隠蔽するために捏造された架空の事件だった!!](https://rapt-plusalpha.com/50120/)

[【東京・武蔵野】外国人住民投票権についてTwitterで反対意見を述べた「金井米穀店」が、抗議者から迷惑デモ行為を受ける](https://rapt-plusalpha.com/50166/)