---
title: "ウェブカメラで撮影した動画を、直接サーバーに送る。"
description: ""
date: "2022-06-29T09:36:13+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "javascript"
menu:
main:
  name: プログラミング
  weight: 2
---
ウェブカメラで撮影した動画を直接サーバーに送る方法を調べてみました。

<!--more-->

ウェブカメラで撮影した動画を録画するためのコードを以下のサイトからコピーして、アップロード機能を追加しました。

[〇 ブラウザからデバイスのカメラを通して動画を録画する](https://qiita.com/niisan-tokyo/items/fa6ff649a9a3312148bc)


```
downloadbutton = document.getElementById('download')
uploadbutton   = document.getElementById('upload')    //uploadbuttonを追加。

（中略）


downloadbutton.addEventListener('click', function(ev) {
    (中略)
}

// download 処理の下に、upload 処理を追加。テンプレートに動画以外のものをアップロードするformタグを追加。

uploadbutton.addEventListener('click', function(ev) {

    var blob = new Blob(record_data, { type: 'video/webm' });

    let form_elem = "#upload_form";
    let data    = new FormData( $(form_elem).get(0) );
    let url     = $(form_elem).prop("action");
    let method  = $(form_elem).prop("method");

    data.set("upload_test", blob, "test.webm");

    $.ajax({
        url : url,
        type: method,
        data : data,
        processData: false,
        contentType: false,
        dataType: 'json'
    }).done( function(data, status, xhr ) {
        if (data.error){
          //エラーの処理
        }
        else{
          //成功時の処理
        }

    }).fail( function(xhr, status, error) {
        console.log(status + ":" + error );
    });

});
```

動的にformを作成する場合。

```
let url = PATH;
let data = new FormData();
data.set("upload_test", blob, "test.webm");

$.ajax({
    url : url,
    type: 'POST',
    data : data,
    processData: false,
    contentType: false,
    dataType: 'json'
(以下略)
```

CSRFトークンについては、下記スクリプトを予め実行しておく。

```
//Ajax実行前にセッションIDを送信するスクリプト
function getCookie(name) {
    var cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++) {
            var cookie = cookies[i].trim();
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}
var csrftoken = getCookie('csrftoken');
function csrfSafeMethod(method) {
    // these HTTP methods do not require CSRF protection
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}
$.ajaxSetup({
    beforeSend: function(xhr, settings) {
        if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
            xhr.setRequestHeader("X-CSRFToken", csrftoken);
        }
    }
});


```
[〇 FormDataをformタグではなく、オブジェクトにキーと値をセットした上でAjax送信](https://noauto-nolife.com/post/javascript-formdata-obj-set/)

その他、CSRFトークンの追加に関する参考サイト。

[〇【Django】JavaScriptで動的に作成されたformにcsrf_tokenを追加する](https://qiita.com/kensussu/items/d8da79325108d3601907)

[〇 Djangoを用いてhtmlからPythonファイルを実行する](https://qiita.com/kkkei257/items/b3292b443699ecfb148f)

##### お薦め

[〇 RAPTブログによって、私たちがどれだけイルミナティによって嘘の情報に洗脳され、本来あるべき幸福を奪われていたかを知った!!（十二弟子・エリカさんの証）](https://rapt-plusalpha.com/46236/)

[〇 エジプト考古学者の「吉村作治」は創価学会員 中国共産党の資金援助によって歴史を捏造し、間違った歴史認識を広める中共のスパイだった!!](https://rapt-plusalpha.com/46209/)

[〇「コロナワクチン接種者の寿命は長くて3年」元ファイザー副社長マイケル・イードン氏の命懸けの告発](https://rapt-plusalpha.com/13241/)

[〇【第26回】ミナのラジオ – “コロナ”は中国共産党の起こしたテロだった!! – ゲスト•RAPTさん](https://rapt-plusalpha.com/43057/)

[〇【完全解明】三浦春馬の死の謎　創価学会と少女売春の闇](https://rapt-plusalpha.com/35993/)

[〇「警告!! 日本は既に中国共産党に乗っ取られている」](https://rapt-plusalpha.com/34533/)