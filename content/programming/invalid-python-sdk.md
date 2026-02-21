---
title: "【PyCharm】Invalid Python SDK"
description: ""
date: "2023-03-30T09:45:21+09:00"
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
先日、ubuntuを20.10から22.04.2にアップグレードした際に、Pycharmで開発中のプロジェクトで「Invalid Python SDK」のエラーがでて困った話。

何も考えずにホイホイとアップグレードして、痛い目に遭いました。

<!--more-->

それまでpython3.8がデフォルトだったものが、python3.10に変更されてしまったことや、削除してはいけないものが削除されてしまったからかもしれませんが、とにかく問題の解決方法を検索しました。

それで、以下のサイトにたどり着き、File > Invalidate Caches / Restart を開き、すべてのチェックボックスをクリックして実行した結果、取り敢えずプロジェクトが動くようになりました。

https://stackoverflow.com/questions/70664467/invalid-python-sdk-in-pycharm

https://stackoverflow.com/questions/31840195/invalid-python-sdk-error-while-using-python-3-4-on-pycharm/45099651#45099651

https://www.jetbrains.com/help/pycharm/invalidate-caches.html

しかし、仮想環境のターミナルでpipコマンドが使えないなど、他にも問題があったので、結局、新たなプロジェクトを立ち上げて、開発中のコードをコピペして事なきを得ました。

python3.8をインストールしたのですが、pathが通っていないとか、まだ分からないことが沢山あるので、今回は自分にできる方法で解決しました。

他にも、venvを消去して、インタープリタから新しいものを追加する方法もあるようです。

{{<youtube g9E3XOMxOpM>}}

&nbsp;

以上です。

##### 参考記事

[Ubuntu 22.04.1 LTS へのアップデートと注意点まとめ](https://zenn.dev/noraworld/articles/ubuntu-22-upgrade)

[Ubuntu 20.04 LTS を 22.04 LTS にアップグレードする](https://tech.uzabase.com/entry/2022/10/05/163458)

##### お薦め

{{<x user="Rapt_plusalpha" id="1641054850787192834">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1641040646801883136">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1641020889759367168">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1640682507019636736">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1640336215055687682">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1640320590015004673">}}
&nbsp;
