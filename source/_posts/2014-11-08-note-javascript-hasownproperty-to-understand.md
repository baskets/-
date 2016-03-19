---
layout: post
title: '#Note, Javascript hasOwnProperty 理解'
date: 2014-11-08 12:49
comments: true
categories:
---
>Everything in JavaScript acts like an object,
 with the only two exceptions being null and undefined.
 [Javascript Garden](http://bonsaiden.github.io/JavaScript-Garden/#object.general)

<!--more-->

在 Javascript裡面，每個東西都像是物件，都會繼承自 Object 這個物件。
特別需注意的是，在使用 for in Loop 的時候須注意會向上搜尋一個物件的 prototype chain。

程式範例:
```
Array.prototype.bar = 1; /* 在 Array function 的 prototype Object 新增一個 'bar' property, 其數值是 1*/
var arr = [1,2,3] # inherit from Arr;
for(var i in arr){
  console.log(i);
}
```
也許我們預期要輸出的結果應該是 (一般我們可能只是要取出陣列的值)
```
1
2
3
```
但實際上結果會是
```
1
2
3
bar
```
這是因為 for in loop 會向上 tranverse prototype chain，由於 arr 繼承自 Array，因此會搜尋到 bar。但在大多數的寫程式情況下，我們並不期望這種情況發生。
這時候 hasOwnProperty() 能夠解決這個問題，他會確認被傳入的屬性 (attribute) 是否是被定義在呼叫此方法 (hasOwnProperty) 的物件下，而非物件的 prototype chain 內，因此我們可以這麼做
```
Array.prototype.bar = 1;
var arr = [1,2,3] # inherit from Arr;
for(var i in arr){
  if (arr.hasOwnProperty(i)){
    console.log(i);
  }
}
```
 或不使用 for in loop，而使用最一般的方法
```
Array.prototype.bar = 1;
var arr = [1,2,3] # inherit from Arr;
for(var i = 0;i < arr.length; i++){
  console.log(i);
}
```
這便會輸出我們預期的 output。
這是我對於 hasOwnProperty 的一點小筆記
