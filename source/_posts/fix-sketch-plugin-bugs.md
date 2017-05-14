---
title: Fixed Sketch-Base64-PNG-Export  
tags: 
- Sketch Plugin  
- CocoaScript  
---

修正 Sketch-Base64-PNG-Export 的錯誤
===

{% asset_img 'github.png' %}


前陣子收到一個很久以前寫的`sketch`外掛
網友通報的bug，掛了很久，直到最近才有空修正

> [Do not copy the code. #3](https://github.com/jawayang/Sketch-Base64-PNG-Export/issues/3)

主要的問題，是因為 CocoaScript 的語法又改了  
所以要替換一下寫法  
主要的問題來自這兩段語法  

```
var rect = [MSSliceTrimming trimmedRectForSlice:layer];
var slice = [MSExportRequest requestWithRect:rect scale:1];
```
改成
```
var rect = [[MSSliceTrimming trimmedRectForLayerAncestry:[MSImmutableLayerAncestry ancestryWithMSLayer:layer]]];
var slice = [[MSExportRequest exportRequestsFromExportableLayer:layer] firstObject];
```
		
其餘的功能就會正常了 :)

