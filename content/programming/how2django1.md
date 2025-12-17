---
title: "for ループで id が重複するとき"
description: ""
date: "2022-04-18T19:12:01+09:00"
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
python に限らないと思いますが、for ループを回すときに、id の重複を防ぐ必要があります。
<!--more-->

例えば、以下のようなモデル、テンプレートがあったとします。

`models.py`
```
class Sample(models.Model):

  id       = models.UUIDField(default=uuid.uuid4, primary_key=True, editable=False )
  comment  = models.CharField(verbose_name="コメント", max_length=200)

```

`template`
```
{% for sample in samples %}
    <form id="test" action="{% # %}" method="">
        (中略)
    </form>
{% endfor %}

```

この form タグの id が test のみだと、for ループを回したときに、id が重複します。

そこで、id を以下のように変更します。

```
  <form id="test_{{ forloop.counter }}">
```

あるいは、uuid を結合します。

```
{% for sample in samples %}
    <form id="test_{{ sample.id }}" action="{% # %}" method="">
        <input id="test_btn" type="button">
    </form>
{% endfor %}
```
`sample.id`は uuid4 を使用しているので、重複する可能性は極めて低いです。

この場合、`jQuery`では、次のようにして form タグの値を取得できます。

```
window.addEventListener("load" , function (){
    $(document).on("click","#test_btn", function(){ sample(  $(this).attr("id").replace("test_","") );});
});

function sample(pk){
    let form_elem = "test_" + pk;

    (以下、略)

```

しかし、テンプレートに渡した `samples` に重複がある場合は、id が重複してしまいます。

そのような時には、次のようにします。
```
{% for sample in samples %}
    <form id="test_{{ forloop.counter }}_{{ sample.id }}" action="{% # %}" method="">
        <input id="test_btn" type="button">
    </form>
{% endfor %}
```

そして、`jQuery`を以下のように変更します。

```
window.addEventListener("load" , function (){
    $(document).on("click","#test_btn", function(){ sample(  $(this).attr("id").replace("test_","") );});
});

function sample(pk){
    var new_pk    = pk.slice(-36);
    let form_elem = "test_" + new_pk;

    (以下、略)

```

また、他の状況で色々と思いついたら記載します。