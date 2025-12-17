---
title: "Page Visibility API でページ離脱時にイベントを発生させる"
description: ""
date: "2022-08-12T05:57:54+09:00"
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
先日、[Intersection-Observer-Api に関する記事](https://progress-advance.com/programming/javascript-intersection-observer-api/)にて、ページ遷移時にイベントを発生させるために`beforeunload`を使用しましたが、ブラウザ間で挙動が異なっていたり、`event.returnValue`を記載しないと期待した通りに動かなかったりしました。

<!--more-->

そこで、Page Visibility API というものを使ってみました。

Page Visibility API は、現在ページが見えているかどうかを調べる機能とともに、文書が表示されたり非表示になったりした時を監視することができるそうです。

ただし、IEとSafariは部分的にしかサポートされていないようです。

そこで、前回のコードの以下の部分を IE, Safari, その他に分けて新たに作り直しました。

変更前。
```
window.addEventListener('beforeunload', function(e) {
    submit(array);　//ページ遷移時に転送
}, false);
```

変更後。
```
if (isSafari || isiPhone || isiPad){
    window.addEventListener('pagehide', () => {
        submit(array);
        console.log("safari");
    });
} else if ( isIE || isIE10 || isIE9 || isIE8 || isIE7 || isIE6){
    window.addEventListener('beforeunload', () => {
        submit(array);
        console.log("IE");
    });
} else {
    document.addEventListener('visibilitychange', () => {
        const state = document.visibilityState;
            if (state === 'hidden') {
            submit(array);
            console.log("その他");
        }
    });
}
```
Safari は試していませんが、Edge と Chrome, Firefox は動きました。

`isSafari`等については、[JavaScriptでUserAgentを使った判別をする](https://cly7796.net/blog/javascript/make-a-determination-using-the-useragent-in-javascript/)を参考にしました。

以下、引用させて頂きます。

```
var ua = navigator.userAgent.toLowerCase();
var ver = navigator.appVersion.toLowerCase();
 
// IE(11以外)
var isMSIE = (ua.indexOf('msie') > -1) && (ua.indexOf('opera') == -1);
// IE6
var isIE6 = isMSIE && (ver.indexOf('msie 6.') > -1);
// IE7
var isIE7 = isMSIE && (ver.indexOf('msie 7.') > -1);
// IE8
var isIE8 = isMSIE && (ver.indexOf('msie 8.') > -1);
// IE9
var isIE9 = isMSIE && (ver.indexOf('msie 9.') > -1);
// IE10
var isIE10 = isMSIE && (ver.indexOf('msie 10.') > -1);
// IE11
var isIE11 = (ua.indexOf('trident/7') > -1);
// IE
var isIE = isMSIE || isIE11;
// Edge
var isEdge = (ua.indexOf('edge') > -1);
 
// Google Chrome
var isChrome = (ua.indexOf('chrome') > -1) && (ua.indexOf('edge') == -1);
// Firefox
var isFirefox = (ua.indexOf('firefox') > -1);
// Safari
var isSafari = (ua.indexOf('safari') > -1) && (ua.indexOf('chrome') == -1);
// Opera
var isOpera = (ua.indexOf('opera') > -1);
 
 
// 使用例
if(isIE) {
    alert('IE');
}
if(isIE6) {
    alert('IE6');
}
if(isIE7) {
    alert('IE7');
}
if(isIE8) {
    alert('IE8');
}
if(isIE9) {
    alert('IE9');
}
if(isIE10) {
    alert('IE10');
}
if(isIE11) {
    alert('IE11');
}
if(isEdge) {
    alert('Edge');
}
 
if(isChrome) {
    alert('Google Chrome');
}
if(isFirefox) {
    alert('Firefox');
}
if(isSafari) {
    alert('Safari');
}
if(isOpera) {
    alert('Opera');
}

(中略)
var ua = navigator.userAgent.toLowerCase();
 
// iPhone
var isiPhone = (ua.indexOf('iphone') > -1);
// iPad
var isiPad = (ua.indexOf('ipad') > -1);
// Android
var isAndroid = (ua.indexOf('android') > -1) && (ua.indexOf('mobile') > -1);
// Android Tablet
var isAndroidTablet = (ua.indexOf('android') > -1) && (ua.indexOf('mobile') == -1);
 
 
// 使用例
if(isiPhone) {
    alert('iPhone');
}
if(isiPad) {
    alert('iPad');
}
if(isAndroid) {
    alert('Android');
}
if(isAndroidTablet) {
    alert('Android Tablet');
}

```

##### 参考記事

[beforeunload のダイアログが出現しないことがある](https://www.bugbugnow.net/2022/01/beforeunload-dialog.html)

[Window: beforeunload イベント](https://developer.mozilla.org/ja/docs/Web/API/Window/beforeunload_event)

[ページのコンテンツから離れたタイミングのブラウザイベントの選び方](https://benevolent0505.hatenablog.com/entry/2020/12/01/093000)

[Page Visibility API](https://developer.mozilla.org/ja/docs/Web/API/Page_Visibility_API)

[Document: visibilitychange event](https://developer.mozilla.org/en-US/docs/Web/API/Document/visibilitychange_event)

[使用してるブラウザを判定したい](https://qiita.com/sakuraya/items/33f93e19438d0694a91d)

##### お薦め記事

[【アメリカ】アップルとグーグルに対し、中国の動画投稿アプリ「TikTok」を削除するよう要求　中国の「侵略ツール」として警告](https://rapt-plusalpha.com/50817/)

[中国人は諜報活動することを法律で義務付けられていた!!　中国の「国家情報法」の恐るべき実態](https://rapt-plusalpha.com/43436/)

[YouTubeが「南京大虐殺はなかった」とする真実の動画を次々と削除　中国共産党の反日工作に加担する創価企業Google](https://rapt-plusalpha.com/50451/)

[「南京大虐殺」は、中国人による日本人虐殺「通州事件」を隠蔽するために捏造された架空の事件だった!!](https://rapt-plusalpha.com/50120/)

[【第30回】ミナのラジオ – X JAPANの闇を暴く　Toshiの洗脳も、hideとTAIJIの死も、中国共産党の犯行だった‼︎ – ゲスト・RAPTさん&エリカさん](https://rapt-plusalpha.com/45160/)

[【第27回】ミナのラジオ – 中国共産党のハニートラップにやられて滅亡に向かう日本 – ゲスト・RAPTさん](https://rapt-plusalpha.com/43634/)