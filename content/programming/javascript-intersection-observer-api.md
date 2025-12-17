---
title: "【JavaScript】Intersection-Observer-Apiを使い、特定の要素が画面に入ったら「id」を取得"
description: ""
date: "2022-08-08T10:30:51+09:00"
thumbnail: 
  src: "img/thumb/check.jpg"
  visibility: 
    - list
categories:
  - "programming"
tags:
  - "javascript"
menu:
main:
  name: プログラミング
  weight: 2
---

特定の要素が画面に表示されたら、その要素の id を取得し、POST で views.py に値を渡す方法について。

<!--more-->

```
<div id="a" class="sample">Sample</div>
<div id="b" class="sample">Sample</div>
<div id="c" class="sample">Sample</div>
<div id="d" class="sample">Sample</div>
<div id="e" class="sample">Sample</div>

<form id="idSetForm" action="{% url '' %}" method="POST">
    {% csrf_token %}
</form>

```

```
window.addEventListener("load" , function (){
    let array = [];
    const options = {
        rootMargin: '-50px',
        threshold : 0.5        // 2022/08/09 threshold を追記しました。
    }
    const check = document.querySelectorAll('.sample');

    const observer = new IntersectionObserver(idCheck, options); // 2022/08/09 options を追記しました。
    check.forEach(check => {
        observer.observe(check);
    });

    function idCheck(entries){

        entries.forEach(entry => {
            if (entry.isIntersecting) {
                let attr = entry.target.getAttribute("id");
                array.push(id);
                observer.unobserve(document.getElementById(attr)); // 要素の監視を解除。

//                要素の監視を解除しない場合。               
//                if ( !array.includes(id) ) {
//                array.push(id);
//                }                
            };
        });
    };

    window.addEventListener('beforeunload', function(e) {
        submit(array);　//ページ遷移時に転送
    }, false);

});

function submit(array){

    let form_elem   = "#idSetForm"; //formを取得するか、javascript で作って転送。

    let data    = new FormData( $(form_elem).get(0) );
    let url     = $(form_elem).prop("action");
    let method  = $(form_elem).prop("method");
    data.set('id_list', JSON.stringify(array));

    for (let v of data.entries() ){ console.log(v); }

    $.ajax({
        url: url,
        type: method,
        data: data,
        processData: false,
        contentType: false,
        dataType: 'json'
    }).done( function(data, status, xhr ) {
        if (data.error){
            console.log("ERROR");
        }
        else{
            console.log("SUCCESS");
        }

    }).fail( function(xhr, status, error) {
        console.log(status + ":" + error );
    });

}
```

views.pyに届いた id_list は、 'id_list': ['["a","b","c"]'] のように id のリストが文字列になっているのでリストに変換。
```
import ast

class idCheckView(views.APIView):
    def post(self,request, *args,**kwargs):      
        id_list = ast.literal_eval(request.data["id_list"])
        for id in id_list:
            print(id)

```

##### 参考記事

[【JavaScript】特定の要素が画面に見えているかどうかを判定する方法【Intersection Observer API】](https://clicklyquickly.com/2021/05/16/javascript-intersection-observer-api/)

[ythonでのeval関数の使い方｜基本的な使い方からコマンド実行まで解説](https://www.fenet.jp/dotnet/column/language/7818/)

##### お薦め記事

[YouTubeが「南京大虐殺はなかった」とする真実の動画を次々と削除　中国共産党の反日工作に加担する創価企業Google](https://rapt-plusalpha.com/50451/)

[【李家の孫正義、人望も人材も失う】ソフトバンクグループ傘下の「ビジョン・ファンド」から幹部クラスの人材流出が深刻化](https://rapt-plusalpha.com/50385/)

[コロナを5類相当へ引き下げるよう求める声が高まるも「5類に落とすと、7〜8万円のコロナ治療薬を3割負担で国民が購入しなければならない」とヤブ医者らが脅し、コロナ利権を確保](https://rapt-plusalpha.com/50486/)

[「習近平」が中国共産党の存続を脅かす企業や国民を弾圧しつづけた結果、中国経済が崩壊、“心肺停止”状態に](https://rapt-plusalpha.com/50396/)

[【北海道札幌市】大病院が続々とコロナワクチン接種の中止を宣言 「禎心会病院」「徳洲会病院」「手稲渓仁会病院」「市立札幌病院」](https://rapt-plusalpha.com/50370/)