---
author: "FHVirus"
title: "FHVirus IO"
date: "2020-10-18"
description: "因為是 FHVirus，所以第一篇就來寫輸入輸出吧。"
tags: [
  "模板",
  "輸入輸出",
  "鴨腸",
]
katex: true
---

因為是 FHVirus，所以第一篇就來寫輸入輸出吧。

<!--more-->

# 正常的輸入輸出

大家都應該要會的東西。

### IOStream

輸出入串流，最適合初學者使用的東西。
不用自己判斷型別，但是大部分時候偏慢。

```cpp
#include<iostream>
int a;
std::cin >> a;
std::cout << a << endl;
```

本人覺得沒有到很好用（尤其是輸出的時候，一堆數字加空格很麻煩），但是可以應付大部分工作（例如 CodeForces 的比賽）。

### Faster IOStream

輸入輸出量很大 ( $10^6$ ) 的時候，直接用 `IOStream` 極有可能會 TLE。這個時候⋯⋯

```cpp
ios_base::sync_with_stdio(0);
cin.tie(0);
cout.tie(0);
```

在 `main()` 一開頭加上這三行，並用 `'\n'`（換行字元） 取代 `endl`（換行 + flush，因為有 flush 所以會很慢），通常就可以過了，而且時間會少很多。
至於為什麼，網路上都有，我懶得寫 OW<)b
也有很多人會在程式開頭加上這兩行：

```cpp
#define OW0 ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
#define endl '\n'
```

其中 `OW0` 可以是任何自己喜歡而且不會與變數函數撞名的東東（例如「艾蜜莉亞我婆」、「8e7好強」之類的）。

### cstdIO

通常比 IOStream 快（有點多）。指定輸出格式時超好用。

```cpp
#include<cstdio>
int a;
scanf("%d", &a);
printf("%d", a);
```

剛開始可能會覺得很麻煩，一旦會了就超方便（指定小數位數、寬度、輸入字串直到指定字元等等）。
懶的寫教學，自己查 reference。

### GetLine

顧名思義，輸入一整行的時候可以用的東西。
輸入都要一行一行讀而且輸入量大的時候， `GetLine` 會比其他方法都快（例題待補）。

```cpp
#include<iostream>
#include<string>
string s;
std::getline(cin, s);
```

### Others

```cpp
#include<cstdio>
char c;
c = getchar();
putchar(c);
puts("Jizz");
```
有時候只有要輸出 `"Yes"` 或 `"No"` 的時候就可以用 `puts()`。
`puts()` 會自己多一個換行（像 Python 一樣）。

另外， `putchar` 、 `getchar` 可以換成 `putchar_unlocked` 、 `getchar_unlocked`，似乎會比較快。

---

# 毒瘤壓常輸入輸出

壓 TopCoder / 怪題要會的。
用來造福（Ｘ）毒害（Ｏ）大眾的東東。

### readchar

從[蛋餅那裡](https://omeletwithoutegg.github.io/2019/12/06/Fast-IO/)出來的。

```cpp
#include<cstdio>
inline char readchar(){
	const int S = 1<<16;
	static char buf[S], *p = buf, *q = buf;
	return p == q and (q = (p = buf) + fread(buf, 1, S, stdin)) == buf ? EOF : *p++;
}
```
其中， `S`  的大小可以自己調整。
`fread` 會回傳實際輸入的大小（大概是位元數吧）。

### the R function

這裡提供兩種版本，方便使用但特殊情況會錯的和正確的。

```cpp
inline int R(){
	int ans = 0; char c = readchar(); bool minus = false;
	while((c < '0' or c > '9') and c != '-' and c != EOF) c = readchar();
	if(c == '-') minus = true, c = readchar();
	while(c >= '0' and c <= '9') ans = ans * 10 + (c ^ '0'), c = readchar();
	return minus ? -ans : ans;
}
```

可以根據題目輸入形式化簡。
不要問我為何用 xor ，在下也不知道。
這個版本在輸入 -2147483648 的時候會爆炸。

```cpp
inline int R(int &a){
	static char c;
	while(((c = readchar()) < '0' or c > '9') and c != '-' and c != EOF);
	if(c == '-'){
		a = 0;
		while((c = readchar()) >= '0' and c <= '9') a *= 10, a -= c ^ '0';
	} else {
		a = c ^ '0';
		while((c = readchar()) >= '0' and c <= '9') a *= 10, a += c ^ '0';
	}
}
```
用了參照及 static ，似乎空間比較小吧。

### the W funciton

筆者自行研發的。使用上有點麻煩，不推薦日常使用。

```cpp
char outbuf[S]; int outp;
inline void W(int n){
	static char buf[12], p;
	if(n == 0) outbuf[outp++] = '0';
	p = 0;
	if(n < 0){
		outbuf[outp++] = '-';
		while(n) buf[p++] = '0' - (n % 10), n /= 10;
	} else {
		while(n) buf[p++] = '0' + (n % 10), n /= 10;
	}
	for(--p; p >= 0; --p) outbuf[outp++] = buf[p];
	outbuf[outp++] = '\n';
	if(outp > S-12) fwrite(outbuf, 1, outp, stdout), outp = 0;
}

int main(){
	// ...do something
	fwrite(outbuf, 1, outp, stdout);
}
```

大概就是把所有輸出寫到一個 buffer 。程式結束前記得把剩下的東西 fwrite 出來喔。

### the RD function

很少很少會用到，但是寫了就丟上來吧。基本上只是為了某題寫了之後就丟在模板裡了。
這個只能讀 0 ~ 1 之間的小數喔。

```cpp
inline double RB(){
	double ans = 0, mult = .1; char c = readchar();
	while(c != '.') c = readchar();
	c = readchar();
	while(c >= '0' and c <= '9') ans += mult * (c - '0'), mult *= .1, c = readchar();
	return ans;
}
```

### the RD, RS... functions

待補。
反正每次寫都是再刻一次，也不算模板。

---

# 毒中之毒

一些怪東東：
- Fast & Furious IO (CodeForces 上的東西)
- `#include<unistd.h>` （壓空間，使用方法近 `fread & fwrite`）

如果有人知道其他神奇毒瘤麻煩告訴我 ><

最後，附上[極簡版](https://pastebin.com/hxB2e6Gf)。

---

10/27 更新，感謝油油的 `cwcodinglife` 提醒我修正 `readchar()`。
