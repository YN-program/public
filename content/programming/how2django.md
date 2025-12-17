---
title: "【django】テンプレートからjQueryに複数の値を渡す方法"
description: ""
date: "2022-04-16T20:49:42+09:00"
thumbnail: "img/send_value.png"
categories:
  - "programming"
tags:
  - "django"
menu:
main:
  name: プログラミング
  weight: 2
---

djangoのテンプレートから`jQuery`に複数の値を渡したいときがあります。その方法を一つ思いついたので、記載しておきます。

<!--more-->

`models.py`では、uuid4を使用しています。
```
class Sample(models.Model):

  id       = models.UUIDField(default=uuid.uuid4, primary_key=True, editable=False )
  comment  = models.CharField(verbose_name="コメント", max_length=200)
  target   = models.ForeignKey("self",verbose_name="コメント先",on_delete=models.CASCADE, null=True, blank=True)

```

`template`
```
<input id="sample" class="sample" type="button" value="{{ sample.target.id }}_{{ sample.id }}">
<label for="sample"></label>
```

`jQuery`に送られてきたvalueはuuid4の連結なので、36文字で前後から切り取って2つの`id`を取り出すことができます。
```
window.addEventListener("load" , function (){
    $(document).on("click",".sample", function(){ sample( $(this).val() );});
});

function sample(pk){
    var target_pk = pk.slice(0,36);
    var pk        = pk.slice(-36);

    (以下、略)
```

他にも、次のような方法があります。
[複数の値をonclick関数からjquery関数に送信](http://ja.uwenku.com/question/p-vanqybsc-kw.html?msclkid=2cc35762bdf011ecaa76b9f86d589604)を参照。

`template`
```
<a href="#" id="button" data-params='hello|world'>click</a>
```

`jQuery`
```
$(function () {
    $("#button").on("click", function() {
        var data = $(this).data('params').split('|');
        console.log(data[0]);
        console.log(data[1]);
    });
});
```

宜しければ、参考にしてください。