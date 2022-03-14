---
title: '如月'
date: 2022-02-09T15:00:28+08:00
# img: "images/image.png"
author: FHVirus
tags:
  - "隨筆"
description: "二月份競程筆記"
katex: true
---

補完 IOICamp 的題目後來繼續刷題了……

<!--more-->

## 關於 Treap

1. 可以從葉子往根爬，複雜度是 $O(\log N)$！
2. 一棵不夠，就開 $N$ 棵！節點可以跑來跑去。
3. 如果是要維護一整個 $1 \sim N$ 的序列的話，那就乾脆讓節點偽指標順便代表編號吧。
4. 想想可不可以用 BIT + 二分搜。

