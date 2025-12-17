---
title: "【JavaScript】ページ移動・更新の検知方法"
description: ""
date: "2022-08-08T09:38:24+09:00"
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
javascript でのページ移動、更新を検知する方法のメモ。
<!--more-->

```
window.addEventListener('beforeunload', function(e) {
    e.returnValue = 'message';
}, false);
```
または、
```
$(window).on('beforeunload', function(e) {
  e.returnValue = 'message';
});
```
[【JavaScript】ページ移動・更新を検知する](https://mimirswell.ggnet.co.jp/blog-230)


また、JavaScript でページが更新されたか、「戻る・進む」されたかを調べる方法。

以下のように、 `performance.navigation.type`に直接アクセスすることは禁止されているとのこと。

```
window.onload = function(){
  var type = performance.navigation.type;
  switch(type){
    case performance.navigation.TYPE_NAVIGATE:
      console.log('通常のアクセス');
      break;
    case performance.navigation.TYPE_RELOAD:
      console.log('更新によるアクセス');
      break;
    case performance.navigation.TYPE_BACK_FORWARD:
      console.log('戻るによるアクセス');
      break;
    case performance.navigation.TYPE_RESERVED:
    default:
      console.log('その他のアクセス');
      break;
  }
};
```

正しくは次の通り。

```
window.onload = function(){
  var perfEntries = performance.getEntriesByType("navigation");
  perfEntries.forEach(function(pe){
    switch( pe.type ){
      case 'navigate':
        console.log('通常のアクセス');
        break;
      case 'reload':
        console.log('更新によるアクセス');
        break;
      case 'back_forward':
        console.log('戻るによるアクセス');
        break;
      case 'prerender':
        console.log('レンダリング前');
        break;
    }
  });
};
```

通常読み込みは navigate、更新は reload、戻る/進むによるページ移動は back_forward、事前読み込みは prerender が返ってくる。

[JavaScriptでページが更新 or 戻る・進むされたかなどを調べる方法](https://pisuke-code.com/js-get-navigation-type/)より