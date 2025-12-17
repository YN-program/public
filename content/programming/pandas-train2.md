---
title: "【Pandas】備忘録-その2-"
description: ""
date: "2022-08-21T22:40:03+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "pandas"
menu:
main:
  name: プログラミング
  weight: 2
---

前回記事の続きで、pandas, matplotlib の使い方の備忘録です。

環境：jupyter notebook

<!--more-->

```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
%matplotlib inline
```
データの読み込み。

```
train= pd.read_csv("C:file/to/train.csv")

train
```
![data](/img/pandas/data.png)
&nbsp;

##### yの折れ線グラフを描く

* 折れ線グラフは plot 関数を使う。

```
train['y'].plot()
```
![data](/img/pandas/plotY.png)
&nbsp;

##### グラフを大きくする

* カッコの中にオプションとしてfigsize=(12,4)と書く。
```
train['y'].plot(figsize=(12,4))
```
![data](/img/pandas/plotYsize.png)
&nbsp;

##### グラフにタイトルを付ける

* カッコの中に、title=""を記載。

```
train['y'].plot(figsize=(12,4), title="y")
```
![data](/img/pandas/plotYtitle.png)
&nbsp;

##### グラフのx軸とy軸に名前を付ける

```
#日本語文字化け対応
from matplotlib import rcParams
plt.rcParams["font.family"] = "MS Gothic"

ax = train['y'].plot(figsize=(12,4), title="y")
ax.set_xlabel("X軸", size=20)
ax.set_ylabel("Y", size=20)
```
![data](/img/pandas/plotYxylabel.png)
&nbsp;

##### trainのyのヒストグラム

```
train['y'].plot.hist()
```
![data](/img/pandas/hist.png)
&nbsp;

##### グリッド線を引く

```
train['y'].plot.hist(grid=True)
```
![data](/img/pandas/hist-grid.png)
&nbsp;

##### ヒストグラム上に平均値、中央値を表す線を引く。

```
train['y'].plot.hist(grid=True)
plt.axvline(x=train['y'].mean(), color="red")
plt.axvline(x=train['y'].median(), color="green")
```
![data](/img/pandas/hist-median.png)
&nbsp;

##### ヒストグラムのアレンジ

```
train['y'].plot.hist(bins=50, color="orange", grid=True, figsize=(12,4), label="pandas-hist")
plt.ylim(0,20)
plt.ylabel('frequency')
plt.xlim(20, 180)
plt.xlabel('Y')
plt.legend(bbox_to_anchor=(1, -0.1), loc='upper right',  borderaxespad=0, fontsize=18)
plt.title('pandas_histgram')
```
![data](/img/pandas/hist-arrange.png)
&nbsp;

##### plt.legend() の loc の候補

* best
* upper right
* upper left
* lower left
* lower right
* right
* center left
* center right
* lower center
* upper center
* center

[matplotlib の legend(凡例) の 位置を調整する](https://qiita.com/matsui-k20xx/items/291400ed56a39ed63462)


##### legend(凡例)について

[Matplotlib plt.legend() | 凡例の位置とスタイル設定完璧ガイド！](https://www.yutaka-note.com/entry/matplotlib_legend)より、ほぼ引用。


新しくsin,cos カーブのグラフを描きます。

```
x = np.linspace(0, 2*np.pi)
y1 = np.sin(x)
y2 = np.cos(x)

plt.plot(x, y1, label="sin(x)")
plt.plot(x, y2, label="cos(x)")
 
plt.legend(loc='upper center', bbox_to_anchor=(0.5, -0.1), ncol=2)
plt.show()
```
![data](/img/pandas/sincos.png)
&nbsp;
```
x = np.linspace(0, 2*np.pi)
y1 = np.sin(x)
y2 = np.cos(x)

fig, ax = plt.subplots()
ax.plot(x, y1, label="sin(x)", color="red")
ax.plot(x, y2, label="cos(x)", color=(1, 0.1, 1, 0.2))
 
plt.legend()
plt.show()
```
![data](/img/pandas/sincos2.png)
&nbsp;

```
locs = ['upper left', 'upper center', 'upper right',
        'center left', 'center', 'center right', 
        'lower left', 'lower center','lower right' ]
 
# 描画領域の調整、サブプロットのレイアウト自動調整
plt.figure(figsize = (8,10), tight_layout=True)
 
for i, loc in enumerate(locs):
  # サブプロット作成
  plt.subplot(4, 3, i+1)
  plt.plot(x, y1, label="sin(x)", color =(1, 0.1, 1, 0.7))
 
  # グラフタイトルの表示
  plt.title(loc)
 
  # 軸ラベルの非表示
  plt.xticks([])
  plt.yticks([])
 
  # 凡例の表示
  plt.legend(loc = loc)
 
plt.show()
```

![data](/img/pandas/legend.png)
&nbsp;

以上になります。

##### お薦め

[【コロナは茶番】中国人スパイの岸田首相、4回目のコロナワクチン接種9日後にコロナ感染　ゴルフ・温泉旅行を満喫、タイミングよく静養に入り夏休み延長](https://rapt-plusalpha.com/51734/)

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/Mina%E3%83%A9%E3%82%B8%E3%82%AA2/06bafba746f29b1106b31819bc48cba1e41690f1?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/04/a1fa0266a675a42f6c3026fc0e127e1e615669ce?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/01/20971499ac41177428aba9520a5309d4eaaa5750?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}