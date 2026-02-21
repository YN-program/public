---
title: "JavaScriptで要素のIDを取得"
description: ""
date: "2022-10-07T15:29:19+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "django"
  - "python"
  - "JavaScript"
menu:
main:
  name: プログラミング
  weight: 2
---
ボタンや要素をクリックしたとき、動画、オーディオの終了時、あるいは `querySelectorall()` を用いて要素の ID を取得する方法のメモ。

<!--more-->

以下のコードは、stack overflow から。

```
<button id="btn1" onClick="reply_click(this.id)">B1</button>
<button id="btn2" onClick="reply_click(this.id)">B2</button>
<button id="btn3" onClick="reply_click(this.id)">B3</button>
    
<script type="text/javascript">
  function reply_click(clicked_id)
  {
      alert(clicked_id);
  }
</script>


```
[onClick to get the ID of the clicked button](https://stackoverflow.com/questions/4825295/onclick-to-get-the-id-of-the-clicked-button)

python のテンプレートで、ID にオブジェクトの uuid を付与して、javascript で uuid を取り出す方法。

```
<button id="btn1_{{ test.id }}" onClick="myFunction(this.id)">B1</button>
<button id="btn2_{{ test.id }}" onClick="myFunction(this.id)">B2</button>
<button id="btn3_{{ test.id }}" onClick="myFunction(this.id)">B3</button>
    
<script type="text/javascript">
  function myFunction(id)
  {
    const target = id.slice(-36);
    console.log(target); // test.id(uuid)
  }
</script>
```

その他、videoタグやaudioタグで、再生終了時や停止時に id を取得。

```
<video id="video" controls onpause="myFunction(this.id)" onended="myFunction(this.id)">

// <audio id="audio" controls onpause="myFunction(this.id)" onended="myFunction(this.id)">

<script>
function myFunction(pk) {
    console.log(pk);
}
</script>

```
`querySelectorall()` を使用して、複数の要素のidを取得。

```
<div id="test1" class="sample">test1</div>
<div id="test2" class="sample">test2</div>
<div id="test3" class="sample">test3</div>
<div id="test4" class="sample">test4</div>

<script>
var t = document.querySelectorAll(".sample");

t.forEach(function(item){
    let id = item.getAttribute('id');
    console.log(id);
});

</script>

```

##### 参考記事

[要素の取得方法まとめ](https://qiita.com/amamamaou/items/25e8b4e1b41c8d3211f4)

##### お薦め

{{<x user="Rapt_plusalpha" id="1578015931372294145">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1578004178852786177">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1577989205573394433">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1577975622366040064">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1577967660134203393">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1577630300594925568">}}
&nbsp;
{{<x user="Rapt_plusalpha" id="1577249469883523072">}}
&nbsp;

{{<youtube FXr2626vR58>}}
&nbsp;

{{<youtube ynhVt5evdDc>}}