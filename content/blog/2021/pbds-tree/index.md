---
title: "pb_ds tree"
date: 2021-02-07T17:58:21+08:00
description: "一篇關於平板電視樹的教學"
tags: 
  - 隨筆
  - 模板
katex: false
draft: false
---

因為寫題會用到就順便寫一篇吧。

<!--more-->

## 什麼是平板電視樹（pb_ds tree）？

黑魔法。

## 它可以幹嘛？

大致上，平板電視樹可以當作 `std::set` 或 `std::map` 使用（以下以當作 `std::set` 用為主），但同時又支援兩種實作很麻煩的操作：

- `find_by_order(k)` ：像陣列一樣回傳第 k 個值。
- `order_of_key(k)` ：回傳 k 是集合裡第幾大。

## 怎麼用？

### 宣告

首先是標頭檔：

```cpp
#include<bits/extc++.h>
using namespace __gnu_pbds;
```

再來是型別宣告：

```cpp
tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update>;
// 好長對吧？分開來看看
tree<
  int,				// 資料型別
	null_type,	// 當作 map 使用的時候要對應什麼資料型態，
							// 要當作 set 就用 null_type
	less<int>,	// key value 要用什麼方式比較
							// 類似 priority_queue 的用法
	rb_tree_tag,// 這顆 tree 要用哪種結構
							// 大多時候會用紅黑樹（rb_tree）
	tree_order_statistics_node_update
    					// 神秘的黑魔法，待補
>;
```



### 使用

直接看[模板題](https://www.luogu.com.cn/problem/P3369)的扣的吧。

```cpp
// Knapsack DP is harder than FFT.
#include<bits/stdc++.h>
#include<bits/extc++.h>
using namespace std;
using namespace __gnu_pbds;
typedef int64_t ll;
template<typename T> using rbt = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;
int32_t main(){
	ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	int n;
	rbt<ll> eek;
	cin >> n;
	for(ll opt, x; n; --n){
		cin >> opt >> x;
		if(opt == 1) eek.insert((x<<20) + n);
		else if(opt == 2) eek.erase(eek.lower_bound(x<<20));
		else if(opt == 3) cout << eek.order_of_key(x<<20) + 1 << '\n';
		else if(opt == 4) cout << (*eek.find_by_order(x-1) >> 20) << '\n';
		else if(opt == 5) cout << (*--eek.lower_bound(x<<20) >> 20) << '\n';
		else if(opt == 6) cout << (*eek.lower_bound((x+1)<<20) >> 20) << '\n';
	}
	return 0;
}
```

### 參考資料

- [C++ STL: Policy based data structures by adamant](https://codeforces.com/blog/entry/11080)
- [某篇 CSDN](https://blog.csdn.net/weixin_30593443/article/details/99322952)
