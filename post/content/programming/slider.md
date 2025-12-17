---
title: "【Javascript】横スライダー(forloop内で使用可)"
description: ""
date: "2022-11-27T18:45:39+09:00"
thumbnail: "img/thumb/slider.png"
categories:
  - "programming"
tags:
  - "Javascript"
menu:
main:
  name: プログラミング
  weight: 2
---

forloop内での横スライダー(自作)。

環境：python

<!--more-->

コードは以下の記事を参考にしました。

[【jQuery】ボタン式の横スライダーを自作する【通販サイト・コンテンツ共有サイトなどに】
](https://noauto-nolife.com/post/javascript-carousel-origin-slider/)

[【Django】テンプレートで数値を使用したforループを実行する方法【レビューの星のアイコン表示などに有効】](https://noauto-nolife.com/post/django-template-integer-for-loop/)


ブログ用に一部省略していますが、一応動きます。

`models.py`

モデル Obj に外部キーで ObjImage が付いている。

```
class Obj
  id = 
  content = 

  def images(self):
    return ObjImage.objects.filter(target=self.id).order_by("-date")

class ObjImage
  id = 
  date =
  image = 
  target = models.Foreignkey(Obj, ***)
```

`views.py`

```
objects = Obj.objects.filter(****).annotate(image_len=Count("objimage"))

//annotateでObjに付いている画像枚数を取得
```

`template`
```
{% for obj in objects %}

  (中略)

  <div class="preview_control_area">
      <div id="previousBtn_{{ forloop.counter }}" class="control_button_hide previous_button" tabindex="0" role="button">
          <i class="fas fa-chevron-left" aria-hidden="true"></i>
      </div>

      <div id="data_preview_area_{{ forloop.counter }}" class="data_preview_area">
          {% for img in obj.images %}
          <div class="d-inline-block align-middle">
              <div class="data_preview_content">
                  <img src="{{  }}" alt="img">
              </div>
          </div>
          {% endfor %}
      </div>
      <div id="nextBtn_{{ forloop.counter }}" class="control_button next_button" tabindex="0" role="button">
          <i class="fas fa-chevron-right" aria-hidden="true"></i>
      </div>
  </div>

  <div class="w-100 mx-auto text-center">
      {% with range=''|center:obj.image_len %}
      {% for r in range %}
          <input id="imgBtn_{{ forloop.counter }}_{{ forloop.parentloop.counter }}" class="imgBtn" type="checkbox" name="imgBtn" value="{{ forloop.parentloop.counter }}" {% if forloop.first %} checked {% endif %}>
          <label class="d-inline-block mx-1 imgBtnLabel" for="imgBtn_{{ forloop.counter }}_{{ forloop.parentloop.counter }}"></label>
      {% endfor %}
      {% endwith %}
  </div>

{% endfor %}

  //{{ forloop.parentloop.counter }} は、親ループのカウンター
  //戻る、進むボタンは、font awesomeを使用しています。
```

`css`

```
/* slider system */

.data_preview_area {
    width:calc(100% - 2rem*2);
    font-size:0;
    margin:0.5rem 0;
    overflow-x:auto;
    white-space:nowrap;
    word-break: break-all;

    /* スクロールバーの除去 Edge,Firefox*/
    -ms-overflow-style: none;
    scrollbar-width: none;
}
/* スクロールバーの除去 Chrome Safari */
.data_preview_area::-webkit-scrollbar {
    display:none;
}
.data_preview_content {
    display:block;
    margin:0 0.5rem;
    font-weight:bold;
    overflow-y:hidden;
    text-decoration:none;
}
.data_preview_content:hover{
    text-decoration:none;
}

.imgBtn { display:none;}
.imgBtnLabel {
    width:0.5rem;
    height:0.5rem;
    border-radius:50%;
    background:#FF93FF;
    opacity:0.5;
    vertical-align: middle;
}
input[type=checkbox].imgBtn:checked + .imgBtnLabel{
    width:0.7rem;
    height:0.7rem;
    background:#FF7FFF;
    opacity:1;
    transition:0.4s;
}

/* slider system Control Area */

.preview_control_area {
    display:flex;
    align-items: stretch;
    margin:0 0.25rem;
}
.control_button_hide,
.control_button {
    width:2rem;
    color:#FF93FF;
    font-size:2rem;
    transition:0.2s;
    display:flex;
    justify-content: center;
    align-items: center;
}
.control_button_hide{ opacity:0.3; }

.control_button:hover {
    color:#FF00FF;
    transition:0.2s;
    cursor:pointer;
}
```

`javascript`

```
window.addEventListener("load" , function (){

  $(document).on("click", ".previous_button" ,function(e){e.stopPropagation(); Scroll($(this).attr("id").replace("previousBtn_","")  ,false); });
  $(document).on("click", ".next_button", function(e){e.stopPropagation(); Scroll($(this).attr("id").replace("nextBtn_",""), true); });

  //e.stopPropagation();は必要に応じて。

});

function Scroll(pk, next){

    //pkは親ループの値

    let target  = $("#data_preview_area_" + pk);
    let imgBtn  = document.querySelectorAll('[value=' + '"' + pk + '"' + ']');  //valueからtarget の checkboxを取得

    let single_width    = target.outerWidth();  //ブラウザ幅
    let l = document.getElementById("previousBtn_" + pk);
    let r = document.getElementById("nextBtn_" + pk);

    for(var i=0; i < imgBtn.length; i++){
        if (imgBtn[i].checked) {
            var childId = imgBtn[i].id.replace("imgBtn_","").replace("_"+pk,""); //checkboxのidから子ループのforloop.counter取得
            if (next){
                if( Number(childId) !== imgBtn.length){
                    target.animate({ scrollLeft:"+=" + String(single_width) } , 300);
                    var ID = String( Number(childId) + 1 );
                    document.getElementById("imgBtn_" + ID + "_" + pk).checked = true;
                    imgBtn[i].checked = false;
                    break;
                }
            } else {
                if ( Number(childId) !== 1 ){
                    target.animate({ scrollLeft:"-=" + String(single_width) } , 300);
                    var ID = String(Number(childId) - 1);
                    imgBtn[i].checked = false;
                    document.getElementById("imgBtn_" + ID + "_" + pk).checked = true;
                    break;
                }
            }
        }
    }

    if (next){
        if ( l.classList.contains("control_button_hide") ){
            l.classList.replace("control_button_hide","control_button");
        }
        if ( imgBtn.length === Number(ID) ){
            r.classList.replace("control_button","control_button_hide");
        }
    } else {
        if (r.classList.contains("control_button_hide")){
            r.classList.replace("control_button_hide","control_button");
        }
        if ( ID === "1" ){
            l.classList.replace("control_button","control_button_hide");
        }
    }
}
```

当ブログにはコメント欄を作っていませんが、ご意見がありましたらサイドバーの twitter のリンクからメッセージを頂けると幸いです。

##### お薦め

[〇 47都道府県が“コロナウイルスは存在しない”と回答した公文書一覧](https://rapt-plusalpha.com/16398/)

[〇【ファイザー社】コロナワクチンがウイルス感染を予防できるかどうかの実験をしていなかったことを認める　世界中の人々が怒りの声](https://rapt-plusalpha.com/56627/)

[〇 悪魔を拝んで人生を破壊する生き方から、神様を拝んで人生を幸福にする生き方へ。](https://rapt-neo.com/?p=26065)

[〇 何をどう祈ればいいのか、具体的な方法をお教えします。自分の人生も世界の運命も変える鍵がここにあります。](https://rapt-neo.com/?p=25977)

[〇 悪魔に打ち勝ち、自分の心身を守る方法。祈って聖霊を受けることが、悪魔を滅ぼす最大の鍵です。](https://rapt-neo.com/?p=25914)

[〇 RAPT有料記事430(2019年12月23日）主と自分の向かう方向が一致し、かつ時が一致したとき、主から豊かに霊感と閃きを受けて、やることなすこと全てがうまくいくようになる。](https://rapt-neo.com/?p=52143)

[〇 RAPT有料記事426(2019年12月7日）真に人生を成功させたいと願うなら、常に主とつながって、沢山の閃きを得られるように努力しなさい。](https://rapt-neo.com/?p=52075)

{{<tweet user="Rapt_plusalpha" id="1596799110517903361">}}
&nbsp;
{{<tweet user="Rapt_plusalpha" id="1596793284286107652">}}
&nbsp;