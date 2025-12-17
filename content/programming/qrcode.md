---
title: "QRコードの作り方 -Python, JavaScript(jQuery)-"
description: ""
date: "2022-06-08T09:47:46+09:00"
thumbnail: "img/qrcode/QRcode.png"
categories:
  - "programming"
tags:
  - "python"
  - "JavaScript"
menu:
main:
  name: プログラミング
  weight: 2
---
Python 又は JavaScript で QR コードを作り、html に表示する方法です。

<!--more-->

##### PythonでQRコード生成

`pip install qrcode`

views.py

```
from PIL import Image
import qrcode
import base64
from io import BytesIO

class sample(views.APIView):
  def get(self,request,*args, **kwargs):
    context = {}
    img   = qrcode.make(request.build_absolute_uri()) #表示サイトのURL
    buffer = BytesIO()
    img.save(buffer, format="PNG")
    context["qr"] = base64.b64encode(buffer.getvalue()).decode().replace("'", "")

    return render(request, "app/sample.html", context)

```

sample.html
```
<img src="data:image/png;base64,{{ qr }}"/>
```

結果、このように表示されます。

![qrcode](/img/qrcode/qrcode1.png)

[【Django】QRコード生成 + HTML表示](https://www.automation-technology.info/index.php/2021/02/08/post-205/) を参照しました。

img タグの data とbase64 の意味はこちら。

>base64 のデータで画像ファイルを表示するには、img 要素の中に記述します。src 属性の値には data: 部と base64, 部があります。data: 部にはメディアタイプを指定し、base64, 部にはデータを指定します。
>
>[base64 のデータで画像ファイルを表示する](https://murashun.jp/article/programming/html/html-img-base64.html)

img タグの {{ qr }} の部分。

>普通に画像を配置する場合は以下のように記述しますが、
>
>`<img src="images/hoge.png" />`
>
>それを次のようにするだけです。
>
>`<img src="data:image/png;base64,xxxxx..." />`
>
>「xxxxx…」はBase64化されたデータの文字列を示しています。
>
>上記の例はPNG画像の場合ですが、JPEGを埋め込む場合は「data:image/png」の部分を「data:image/jpeg」としてください。
>
>[画像をBase64でHTMLファイルに直接埋め込む方法](https://edge.sincar.jp/web/base64-inline-image/)

QRコード内に画像を埋め込む方法。

```
from PIL import Image
import qrcode
import base64
from io import BytesIO
from qrcode.image.styledpil import StyledPilImage

class sample(views.APIView):
  def get(self,request,*args, **kwargs):
    context = {}

    qr = qrcode.QRCode(error_correction=qrcode.constants.ERROR_CORRECT_L)
    qr.add_data(request.build_absolute_uri())
    qr.make(fit=True)
    img = qr.make_image(
        image_factory=StyledPilImage,
        embeded_image_path="./media/nikoniko.png")
    buffer = BytesIO()
    img.save(buffer, format="PNG")
    context["qr"] = base64.b64encode(buffer.getvalue()).decode().replace("'", "")

    return render(request, "app/sample.html", context)
```

sample.html
```
<img src="data:image/png;base64,{{ qr }}"/>
```
結果、このように表示されます。

![QRcode](/img/qrcode/QRcode.png)

他、エラーレベルを調整したり、色を変更することもできます。

https://github.com/lincolnloop/python-qrcode


##### JavaScript(jQuery)でQRコード生成

```
<html>
<head>
    <script src="js/jquery-3.4.1.min.js"></script> 
    <script src="js/jquery.qrcode.min.js"></script> 
</head>
<body>
    <button type="button" id="qrcode"><span id="qrAction">QRコード</span></button>
    <div id="qrcode_area"></div>
</body>
</html>
```
jQuery

```
window.addEventListener("load" , function (){
    $("#qrcode").on('click', function() { qrcode(); });
});

function qrcode(){
  //表示されているページのURL
    $(document.body).append("<textarea id=\"copyTarget\" style=\"position:absolute; left:-9999px; top:0px;\" readonly=\"readonly\">" +location.href+ "</textarea>");
    let qrtext      = document.getElementById("copyTarget");
    let utf8qrtext  = unescape(encodeURIComponent(qrtext.value));
    $("#qrcode_area").html("");
    $("#qrcode_area").qrcode({width:160,height:160,text:utf8qrtext});
    qrtext.parentNode.removeChild(qrtext);
}
```

参照：

[JavaScript(jQuery)でQRコードを表示させる](https://noauto-nolife.com/post/javascript-qrcode/)

[JavaScriptでQRコードを作成【jQuery】](https://onoredekaiketsu.com/create-qr-code-with-javascript/)

`escape/unescape`、`encodeURIComponent/decodeURIComponent`について。

https://stackoverflow.com/questions/619323/decodeuricomponent-vs-unescape-what-is-wrong-with-unescape

https://www.sejuku.net/blog/24128