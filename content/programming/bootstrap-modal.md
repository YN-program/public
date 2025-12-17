---
title: "BootstrapのModalの使い方"
description: ""
date: "2022-06-02T08:38:40+09:00"
thumbnail: ""
categories:
  - "programming"
tags:
  - "Bootstrap"
menu:
main:
  name: プログラミング
  weight: 2
---
Bootstrap の Modal を使うと、簡単にモーダルウィンドウを表示することができます。

<!--more-->

![sample1](/img/bootstrap/sample1.png)

```
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary btn-lg" data-toggle="modal" data-target="#myModal">
  Launch demo modal
</button>

<!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="myModalLabel">Modal title</h4>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```
https://getbootstrap.com/docs/3.4/javascript/

注意事項としては、複数のモーダルを開くことはサポートされていないことや、モーダルのコードをドキュメントの最上位に配置することなど。

>##### Multiple open modals not supported
>
>Be sure not to open a modal while another is still visible. Showing more than one modal at a time requires custom code.
>
>##### Modal markup placement
>
>Always try to place a modal's HTML code in a top-level position in your document to avoid other components affecting the modal's appearance and/or functionality.
>
>##### Mobile device caveats
>
>There are some caveats regarding using modals on mobile devices. See our browser support docs for details.

モーダルを他の要素の中に入れると、モーダルウィンドウが影に隠れてしまう事があるようです。

[【bootstrap】モーダルのダイアログが影に隠れちゃう現象](https://www.techblogchan.com/entry/2019/10/10/110000)

モーダルにデータを渡したいときは、ボタンに`data-whatever=""`を記載します。
以下、 https://getbootstrap.com/docs/3.4/javascript/ より。

```
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" data-whatever="@mdo">Open modal for @mdo</button>
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" data-whatever="@fat">Open modal for @fat</button>
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" data-whatever="@getbootstrap">Open modal for @getbootstrap</button>
...more buttons...

<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="exampleModalLabel">New message</h4>
      </div>
      <div class="modal-body">
        <form>
          <div class="form-group">
            <label for="recipient-name" class="control-label">Recipient:</label>
            <input type="text" class="form-control" id="recipient-name">
          </div>
          <div class="form-group">
            <label for="message-text" class="control-label">Message:</label>
            <textarea class="form-control" id="message-text"></textarea>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Send message</button>
      </div>
    </div>
  </div>
</div>
```


```
$('#exampleModal').on('show.bs.modal', function (event) {
  var button = $(event.relatedTarget) // Button that triggered the modal
  var recipient = button.data('whatever') // Extract info from data-* attributes
  // If necessary, you could initiate an AJAX request here (and then do the updating in a callback).
  // Update the modal's content. We'll use jQuery here, but you could use a data binding library or other methods instead.
  var modal = $(this)
  modal.find('.modal-title').text('New message to ' + recipient)
  modal.find('.modal-body input').val(recipient)
})
```

結果として、このようになります。

![sample2](/img/bootstrap/sample2.png)

![sample3](/img/bootstrap/sample3.png)

次いで、モーダルに web ページの URL をクリップボードにコピーするボタンを設置したい場合です。

詳細はこちら。[JavaScriptでwebページのURLをクリップボードにコピーするボタンを設置](https://blanche-toile.com/web/js-copy-clipboard)

`modal-body`に、以下のボタンを設置します。

```
<div class="modal-body">
 <button type="button" id="copy-page"><span id="cAction">URLをコピー</span></button>
</div>
```

`JavaScript`

```
<script>
document.getElementById("copy-page").onclick = function() {
  $(document.body).append("<textarea id=\"copyTarget\" style=\"position:absolute; left:-9999px; top:0px;\" readonly=\"readonly\">" +location.href+ "</textarea>");
  let obj = document.getElementById("copyTarget");
  let range = document.createRange();
  range.selectNode(obj);
  window.getSelection().addRange(range);
  document.execCommand('copy');
  document.getElementById("cAction").innerHTML = "コピーしました";
};
</script>
```

便利で、助かります。

(2022/06/03 20:10 追記)

上記の`execCommand()`は廃止されているようなので、別の方法を使わなければならないようです。

こちらの記事に、Clipboard API を使う方法、window.clipboardDataを使う方法、Clipboard APIを利用できない場合のexecCommandメソッドを使う方法が記載されています。

[JavaScriptで任意のテキストをデバイスのクリップボードにコピーする方法。](https://memo.ag2works.tokyo/post-2504/)
