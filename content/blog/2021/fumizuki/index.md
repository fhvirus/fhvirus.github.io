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
有一些小技巧，看到賺到（？

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

## 關於邊界

1. 記得檢查自己假定的東西有沒有成立。  
例如，假定有解但是其實不一定有，或是假定字串長度大於 1 卻有可能等於 1 。
2. 當找不到 bug 的時候，換一種寫法吧。

## 關於數論及微唬爛

1. [這裡](https://gist.github.com/dario2994/fb4713f252ca86c1254d)有個有趣的東西。也許可以提供數論題唬爛的想法。  
稍微整理一下之後可以發現大概就是 $O(\sqrt[3]{x})$ 的趨勢。

|    範圍     | 因數個數上界 |   有最多因數的數   |
| :---------: | :----------: | :----------------: |
|  $10 ^ 9$   |     1344     |     735134400      |
| $10 ^ {12}$ |     6720     |    963761198400    |
| $10 ^ {18}$ |    103680    | 897612484786617600 |

## 關於資結實作

1. 要實作多棵相同資結的時候（如線段樹、Trie 的時候），可以先開一個 pool 再用偽指標（很好寫！）  
如果怕 Memory leak 的話也可以開一個 `vector<int>` 紀錄被 delete 的指標們。
2. Trie 真的不難寫。
3. 一個小技巧：假設有一個序列題，要一個一個拔掉，拆成一個一個子區間，而子區間可以用資結維護的時候，  
不妨考慮反過來變成一個一個加入，資結可以啟發式合併做！

## 關於中毒

例題：[CSAcademy Online XorMax](https://csacademy.com/contest/archive/task/online_xormax/statement/)

1. 好好觀察，複雜度先好好估，估完有點爛要強迫自己再想一下。
2. 想到可怕的東東也是中毒徵兆之一。
3. 不要分塊、莫隊中毒，真的，序列題還可以有更多好性質。
4. 去練一下莫隊。赫然發現自己寫不出來乾淨實作。

## 關於定方向

例題：[CEOI 2017 Day 1 One Way Streets](https://csacademy.com/contest/archive/task/one-way-streets/statement/)
[巧妙作法 by Zi-Hong Xiao](https://csacademy.com/submission/3119849/)

1. 仔細想好，在 DFS 的時候可以一起別的事。BCC Tarjan 其實不一定要把樹建出來。
2. 在做事的時候要想好「要用的性質」而不是先套演算法。
3. 在樹上定方向的時候，可以直接 `++u, --v`，再樹上 DP。因為處理完子樹就知道現在節點的方向了！
4. XZH 真的特別強Orz。

## 關於唬爛

例題：[CSAcademy Odd Palindromes](https://csacademy.com/contest/archive/task/odd-palindromes/statement/)

1. 複雜度 $O(26N), N \le 10 ^ 6$ 的字串題居然只要 40 ms ？（輸入用 `getline`）
