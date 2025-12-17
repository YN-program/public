---
title: "【JavaScript】演算や要素の有無の確認の備忘録"
description: ""
date: "2022-08-19T19:43:47+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Javascript"
menu:
main:
  name: プログラミング
  weight: 2
---
タイトルの通り、JavaScript での演算の方法や、要素の有無の確認の備忘録です。

JavaScript で数値の足し算をするとき、文字列が含まれていると上手くいきません。

<!--more-->

文字列の「1」と「1」を足すと「11」という答えが返ってきます。

これは、文字列には足し算があり、文字列同士を足すと文字列の連結と見なされるからです。

解決方法としては、parseIntを使って、文字列を整数に変換します。

```
str1 = "1";
str2 = "1";

num1 = parseInt(str1);
num2 = parseInt(str2);

// num1 + num2 の結果が 11 ではなくて 2 になる。
```
掛け算や割り算、引き算には文字列同士の計算はないので、暗黙のルールとして数値型に変換されて計算されます。

parseInt()の他にも、Number(), parseFloat()を使って文字列を数値に変換することができます。

> Number()は簡単に使えて便利ですが、数字以外を含む文字列を引数に指定するとNaNを返すので注意が必要です。
> 
> parseInt()は文字列を整数に変換し、実数の場合は小数点以下を切り捨てます。また、第2引数で変換時の基数を指定することができ、省略した場合は10進数として変換します。基数が10の場合は引数に数字以外の文字が含まれていても無視します。
> 
> parseFloat()は文字列を実数に変換します。引数に数字以外の文字が含まれている場合は無視します。
> 
> [JavaScriptで文字列を数値に変換する：Number(), parseInt(), parseFloat()](https://uxmilk.jp/11582)

```
Number('12345');          // 12345
Number('12345abc');       // NaN
parseInt('12345', 10);    // 12345（10進数の整数）
parseInt('ff', 16);       // 255(16進数の整数）
parseInt('2.9', 10);      // 2（小数点以下を切り捨てる）
parseInt('12345abc', 10); // 12345（数字以外は無視）
parseFloat('2.9');        // 2.9（実数）
parseFloat('12345abc');   // 12345（数字以外は無視）
```

***

##### JavaScript/jQueryで要素の存在の確認方法

```
document.getElementById("id") != null
$("selector")[0]
$("selector").get(0)	
$("selector").size()
$("selector").length
$("selector").is("*")

```

[[JS][jQuery] 要素の存在を確認する6通りのコードと実行速度](http://kihon-no-ki.com/check-existence-element-by-javascript-and-jquery)

または、

```
//要素が存在するかどうか

var elem = document.getElementById("test");

if (elem === null){
	// 存在しない場合の処理
} else {
	// 存在する場合の処理
}

//要素が存在する場合のみ

if (elem !== null){
	// 存在する場合の処理
}

```

[JavaScript: 要素の存在を確認する](https://step-learn.com/article/javascript/164-element-existence.html)

##### 演算子 === と == , !== と != の違い

> ==(等値演算子)は左右の値が同じ場合にtrueを返す。 型変換はJavaScript側で自動的に行ってくれる。
> 
> ===(同値演算子)の方が厳密な比較をする。 値だけでなく、型も同じ場合にtrueを返す。
> 
> !=(不等演算子)は==(等値演算子)の逆で、 !==(非同値演算子)は===(同値演算子)の逆となる。
> 
> 条件式で=(代入演算子)を使ってしまうとバグになる。`if (a=b) 処理`
> 
> [JavaScript: ==(等値演算子)と===(同値演算子)の違い](https://step-learn.com/article/javascript/011-equal.html)


[JavaScriptでの基本演算 — 数値と演算子](https://developer.mozilla.org/ja/docs/Learn/JavaScript/First_steps/Math)によると、`===`や`!==`を使ったほうが良いとのこと。

> メモ: もしかしたら == や != といった演算子を同値かどうかの判定に使用する人を見かけることがあるかもしれません。これらも JavaScript の有効な演算子ですが、=== や !== とは異なります。前者のバージョンは値が同様であるかを判定しますが、データ型が同様かは判定しません。後者は厳格なバージョンで値とデータ型の両方を判定します。厳格なバージョンはエラーとなることが少ないため後者を使用することをお勧めします。


##### お薦め記事

[子供のために真心から祈ることで、子供自ら専門分野を極め、専門学校の先生から教える側になってほしいとまで言われた不思議な体験（十二弟子・ミナさんの証）](https://rapt-plusalpha.com/51572/)

[【コロナウイルスは存在しない】日本人がマスク着用とワクチン接種を徹底した結果、感染者数が4週連続で世界一に BCGがコロナ感染の予防に効果的との新たなデマも拡散される](https://rapt-plusalpha.com/51578/)

[【グローバルダイニング裁判】東京都が実施した営業時間の短縮命令を「違法」とした判決が確定](https://rapt-plusalpha.com/51375/)

&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/8/2b598c7845f10b8f0a550aa65a60f47f973b80b3?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/03/8f93e98f37bf3eb81aafe72ad2e7f0423a4560a6?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}
&nbsp;

{{<file>}}
<iframe id="odysee-iframe" width="560" height="315" src="https://odysee.com/$/embed/7/b54bf0d4d8050d14751fbb184a7b3357745a7169?r=7e4zsKjBG4r6s3UZ6JSSrPioHVwscbXX" allowfullscreen></iframe>
{{</file>}}