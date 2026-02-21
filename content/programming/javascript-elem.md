---
title: "【Javascript】ラジオボタン・チェックボックスのチェックの判定や、要素のid属性取得・変更、前後の要素の取得など"
description: ""
date: "2022-11-26T18:53:51+09:00"
thumbnail:
  src: "img/thumb/programming.jpg"
  visibility:
    - list
categories:
  - "programming"
tags:
  - "Javascript"
menu:
main:
  name: プログラミング
  weight: 2
---

Javascriptでのラジオボタンやチェックボックスのチェックの判定や、要素のid属性の取得・変更、ある要素の前後の要素を取得する方法などのまとめです。

<!--more-->

〇どのラジオボタンがチェックされているか調べる方法。

```
<form id="Form" name="Form">
  <label><input type="radio" name="radioBtn" value="1">1</label><br>
  <label><input type="radio" name="radioBtn" value="2">2</label><br>
  <label><input type="radio" name="radioBtn" value="3">3</label><br>
  <label><input type="radio" name="radioBtn" value="4">4</label><br>
  <input type="button" id="checkButton" value="チェック">
</form>
<div id="result">結果：</div>
~~~~~~~~

<script>
window.onload = function(){
  document.getElementById("checkButton").onclick = function(){
    var radioList = document.getElementsByName("radioBtn");
    var str = "none";
    for(var i=0; i<radioList.length; i++){
      if (radioList[i].checked) {
        str = radioList[i].value;
        break;
      }
    }
    document.getElementById("result").innerHTML = str;
  }
}
</script>

```

http://www.openspc2.org/reibun/JavaScript_technique/sample/03_form/019/

[ラジオボタンのチェック状態を取得する (JavaScript プログラミング)
](https://www.ipentec.com/document/javascript-get-radiobutton-value)


〇JavaScript または jQuery でラジオボタンをチェックする方法

```
<div id="radio-area">
    <label><input type="radio" id="radio1" name="radioBtn" value="1">1</label><br>
    <label><input type="radio" id="radio2" name="radioBtn" value="2">2</label><br>
    <label><input type="radio" id="radio3" name="radioBtn" value="3">3</label><br>
    <label><input type="radio" id="radio4" name="radioBtn" value="4">4</label><br>
</div>
~~~~~

//javacript

//idから
document.querySelector('#radio1').checked = true;

//valueから
document.querySelector('#radio-area > [value="2"]').checked = true;	

//valueの重複がない場合、valueのみで要素を選択可能
document.querySelector('[value="3"]').checked = true;

//jQuery（バージョン1.6以上）
$("#radio4").prop("checked", true);

//jQuery（バージョン1.6未満）
$("#radio4").attr('checked', 'checked');   

//valueから
$("input[name=radioBtn][value='1']").prop("checked",true);
```

〇javascriptで要素のid属性を取得、変更

```
var element = ***;

var elem_id = element.id;
//要素にid属性がない場合は空の文字列が返る

//id属性の変更
element.id = "sample"; 
```
[Element.id - id属性を取得、変更する](https://syncer.jp/javascript-reference/element/id)

〇javascriptで前の要素を取得する　― previousElementSibling ―

```
<input value="1">
<input id="target" value="2">
<input value="3">

~~~~~

var element = document.getElementById( "target" ) ;

var pre = element.previousElementSibling ;
console.log(pre.value); //結果: 1

//previousSibling という似たものがある。詳細は以下のリンク。
```

[Element.previousElementSibling - 前の要素を取得する](https://syncer.jp/javascript-reference/element/previouselementsibling)

〇javascriptで次の要素を取得する　― nextElementSibling  ―


```
<input value="1">
<input id="target" value="2">
<input value="3">

~~~~~

var element = document.getElementById( "target" ) ;

var next = element.nextElementSibling ;
console.log(next.value); //結果: 3

//nextSibling  という似たものがある。詳細は以下のリンク。
```

[Element.nextElementSibling - 次の要素を取得する](https://syncer.jp/javascript-reference/element/nextelementsibling)


〇複数のチェックボックスのチェックの有無を確認する方法

```
<div>
  <input type="checkbox" id="checkbox1" name="checkbox-multi">
  <label for="checkbox1">チェックボックス1</label>
  <input type="checkbox" id="checkbox2" name="checkbox-multi">
  <label for="checkbox2">チェックボックス2</label>
  <input type="checkbox" id="checkbox3" name="checkbox-multi">
  <label for="checkbox3">チェックボックス3</label>
  
  <p>チェックボックス1の状態：<span class="output-status">未選択</span></p>
  <p>チェックボックス2の状態：<span class="output-status">未選択</span></p>
  <p>チェックボックス3の状態：<span class="output-status">未選択</span></p>
</div>

<script>
window.addEventListener('DOMContentLoaded',function(){
  const outputStatus = document.getElementsByClassName('output-status');
  const checkboxMulti = document.getElementsByName('checkbox-multi');
  
  for(let i = 0; i < checkboxMulti.length; i++){ // ←重要なポイント！
    checkboxMulti[i].addEventListener('change',function(){ // ←重要なポイント！
      if(this.checked) {
        outputStatus[i].textContent = "選択済";
      } else{
        outputStatus[i].textContent = "未選択";
      } 
    });
  }
});
</script>
```

[【JavaScript】チェックボックスにチェックが入っているか判定する方法](https://web-engineer-wiki.com/javascript/checkbox-checked/)

今回の記事は、参考記事から殆ど引用させて頂きました。

##### お薦め

{{<x user="Rapt_plusalpha" id="1596457823776346112">}}
&nbsp;

{{<x user="Rapt_plusalpha" id="1591416188713992194">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1596456009555329024">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1596453699081363457">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1596437032913731584">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1596434766118912000">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1595380387458347008">}}
&nbsp;
