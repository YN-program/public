---
title: "for文でのモーダルダイアログ"
description: ""
date: "2022-06-03T09:29:10+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "modaldialog"
  - "django"
menu:
main:
  name: プログラミング
  weight: 2
---
for文(django)でモーダルダイアログを使う事があるので、メモ。

<!--more-->

基本形は、[CSS3とHTML5だけでモーダルダイアログを作る【JS不要】](https://noauto-nolife.com/post/css3-modal-dialog/)から引用しました。

for文内ではない、モーダルダイアログの基本形。

`HTML`
```
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>Hello World test!!</title>

    <link rel="stylesheet" href="style.css">
</head>
<body>

    <label class="modal_label" for="modal_chk">新規作成</label>

    <input id="modal_chk" class="modal_chk" type="checkbox">
    <div class="modal_body">
        <label class="modal_bg" for="modal_chk"></label>
        <div class="modal_content"></div>
    </div>

</body>
</html>
```
`CSS`

```
body { margin:0; }

.modal_label {
    border:solid 0.1rem black;
    padding:0.25rem;
    cursor:pointer;
}
.modal_body { display:none; }
.modal_chk { display:none; }
.modal_bg {
    position:fixed;
    top:0;
    left:0;
    width:100vw;
    height:100vh;
    background:rgba(0,0,0,0.8);
    cursor:pointer;
}
.modal_content {
    position:absolute;
    top:50%;
    left:50%;
    width:80%;
    height:80%;
    transform:translate(-50%,-50%);
    background:white;
}
input[type="checkbox"]#modal_chk:checked + .modal_body { display:block; }

```

次に、django、for文内でのモーダルダイアログ。

`HTML`

```
<div style="position:relative;">
    {% for s in samples %}
    <label id="modal_label_{{ s.id }}" class="modal_label" for="modal_chk_{{ s.id }}">新規作成</label>

    <input id="modal_chk_{{ s.id }}" class="modal_chk" type="checkbox">
    <div id="modal_body_{{ s.id }}" class="modal_body">
        <label id="modal_bg_{{ s.id }}" class="modal_bg" for="modal_chk_{{ s.id }}"></label>
        <div class="modal_content"></div>
    </div>
    (以下、略)
    {% endfor %}
</div>    
```
`jQuery`

```
window.addEventListener("load" , function (){

    $(document).on('click',".modal_label", function() { modal_open( $(this).attr("id").replace("modal_label_","") ); });
    $(document).on('click',".modal_bg", function() { modal_close( $(this).attr("id").replace("modal_bg_","") ); });

});

function modal_open(pk) {
    $('#modal_body_' + pk).css('display', 'block');
}

function modal_close(pk) {
    $("#modal_body_" + pk).css('display', 'none');
}

```

後は、モーダル内に必要なものを設置して、CSS で装飾すると動きます。