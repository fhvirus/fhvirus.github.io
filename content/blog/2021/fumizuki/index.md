---
title: '文月'
date: 2021-07-08T11:51:21+08:00
# img: "images/image.png"
author: FHVirus
tags:
  - "隨筆"
description: "七月份競程筆記"
katex: true
---

一些寫題的時候的小筆記，不夠撐起一整篇題解的東西應該會放在這裡。

<!--more-->

## 關於枚舉

例題：[IZhO 2019 Intergalatic Ship](https://oj.uz/problem/view/IZhO19_xorsum)

1. 想辦法寫出更好觀察的式子（數式展開！）
2. 調換枚舉順序（$\sum$ 可以內外換！）

## 關於 DP 優化

例題：[TIOJ 1347 希爾伯特的書架](https://tioj.ck.tp.edu.tw/problems/1347)

1. 斜率優化其實是可以 $O(1)$ 算出轉移區間邊界的 1D/1D 優化？（或至少感覺很像）
2. [這筆](https://pastebin.com/zni6qbFX)會錯（1D/1D 不能直接看頭尾）的原因：  
頭尾符合單調的情況下，可能有更好的轉移點卡在中間；  
但是只看頭尾的話就不會被戳到，也就不會把前面的推掉。  
還是要好好 $O(n \log n)$ 維護轉移區間才行。
