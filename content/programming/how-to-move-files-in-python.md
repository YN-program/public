---
title: "Pythonでディレクトリ内のファイルをモデルに保存する方法"
description: ""
date: "2022-07-08T09:35:31+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "python"
menu:
main:
  name: プログラミング
  weight: 2
---
本題の前に、前回記事にてサーバーにアップロードした webm ファイルを ffmpeg を使って mp4 に変換する際に、`subprocess.call('', shell=True)`のコードを紹介しました。

[ウェブカメラで撮影した動画(webmファイル)のdurationがInfinityになる問題](https://progress-advance.com/programming/webm-duration-infinity/)

<!--more-->

前回記事のコードはこちらになります。

```
import os
import subprocess as sp

input_file = ''
file_name = os.path.splitext(os.path.basename(input_file))[0]

cmd_list = ['ffmpeg', '-i', input_file, file_name + '.mp4']
cmd = ' '.join(cmd_list)
sp.call(cmd, shell=True)
```
subprocess における`shell=True`と`shell=False`の挙動が違うようですが、`shell=True`よりも`shell=False`のほうが安全なようなので、次のように変更しました。

```
input_file = ''
file_name = os.path.splitext(os.path.basename(input_file))[0]

cmd_list = ['ffmpeg', '-i', input_file, file_name + '.mp4']
sp.call(cmd, shell=False)
```
以下の記事によると、subprocess に渡す args パラメータは、str もしくは list で、一般には list が推奨とのこと。

また、`shell=True`のときは str、`shell=False`のときは list を使用するようです。

[subprocess で shell=True でリストを与えたときの挙動](https://qiita.com/yoichi22/items/5afa8b3b39c723acb359)

[Python: subprocess call with shell=False not working](https://stackoverflow.com/questions/25465700/python-subprocess-call-with-shell-false-not-working)

それで本題ですが、ファイルの変換にてmp4ファイルが作られますが、これを MoviePyで処理をした後、モデルに保存したい。

MoviePy で処理してできたファイルは、一時的なディレクトリに保存されているものとします。

```
from django.core.files import File

(略)

# webmファイルと一緒に送られてきたデータをコピー
copied = request.POST.copy()
copied_file = request.FILES.copy()

copied["user"] = request.user

f = open('mp4ファイルのパス',　mode='rb')

copied_file["video"] = F(f)

form = SampleForm(copied, copied_file)
if form.is_valid():
    form.save()
else:
    print("ERROR")
f.close()
    
# 一時的に保存されたファイルは`os.remove()`で削除する。
```

[【django】画像アップロードして保存前にリサイズやサニタイズ処理を実行する方法](https://freeheroblog.com/resize-img/)


from django.core.files import File の File オブジェクトについて。

> Internally, Django uses a django.core.files.File instance any time it needs to represent a file.
> 
> Most of the time you’ll use a File that Django’s given you (i.e. a file attached to a model as above, or perhaps an uploaded file).
> 
> If you need to construct a File yourself, the easiest way is to create one using a Python built-in file object:
>```
> from django.core.files import File
> 
> #Create a Python file object using open()
> f = open('/path/to/hello.world', 'w')
> myfile = File(f)
>```
>
> https://django.readthedocs.io/en/stable/topics/files.html

これで、ウェブカメラで撮影した動画をサーバーに保存できるようになりました。

##### お薦め記事

現在、大変注目を浴びている楽天の三木谷会長ですが、竹中平蔵と同じ穴の狢だと暴かれました。

是非、こちらの記事をご覧ください。

[楽天・三木谷会長の周辺で次々と有名企業の社長が不審死 中国共産党による“ハニートラップ”の実態を隠蔽するため暗殺された可能性大](https://rapt-plusalpha.com/46834/)

[六本木クラブのオーナーが未成年を含む外国人女性を楽天・三木谷に斡旋していたことが発覚　ウクライナ女性を使った“ハニートラップ”を仕掛ける中共（江沢民派）のスパイ「楽天・三木谷」](https://rapt-plusalpha.com/46779/)

[中国海軍が尖閣水域に進入するも、日本政府は「懸念」を表明し抗議するのみ 記者会見した木原誠二官房副長官は、三木谷ルームのパーティに参加していたことが発覚](http://rapt-plusalpha.com/46625/)

##### おすすめ動画

{{<youtube ebyUeIGqgds>}}
&nbsp;
{{<youtube PQv2PlyjhbE>}}
&nbsp;
{{<youtube 6W9hPx3hH5s>}}