---
author: "FHVirus"
title: "Template"
date: "2020-10-26"
description: "偶爾把一些重要的東西丟上來吧。"
tags: [
	"模板",
]
draft: true
---

到目前為止（`2020/10/26`）都在用 `8e7` 的模板，想說自己也來寫一份好了。
會盡量用 STL、寫一份乾淨易懂的模板。

<!--more-->

# 資料結構
---

## BIT (Fenwick Tree)

```cpp
struct BIT{
    int n, val[N];
    inline void init(int _n){
        n = _n;
        for(int i = 0; i <= n; ++i)
            val[i] = 0;
    }
    inline void init(int _n, int v[]){
        n = _n;
        for(int i = 1; i <= n; ++i)
            val[i] = 0;
        for(int i = 1; i <= n; ++i){
            val[i] += v[i];
            if(i + (i & -i) <= n)
                val[i + (i & -i)] += val[i];
        }
    }
    inline void modify(int p, int v){
        for(; p <= n; p += p & -p) val[p] += v;
    }
    inline int query(int p){
        int ans = 0;
        for(; p > 0; p -= p & -p) ans += val[p];
        return ans;
    }
    inline int query(int l, int r){ // [l, r)
        return query(r - 1) - query(l - 1);
    }
    inline int search(int v){
        int sum = 0, p = 0;
        for(int i = 31 - __builtin_clz(n); i > 0; --i)
            if(p + (1 << i) <= n and sum + val[p + (1 << i)] < v)
                p += 1 << i, sum += val[p];
        return p + 1;
    }
} bit;
```

## Sparse Table

```cpp
const int L = 17 + 1, N = 1e5;
int val[L][N];
inline void initSparse(){
    for(int i = 0; i < n; ++i) val[0][i] = R();
    for(int l = 1; l < L; ++l)
        for(int i = 0; i + (1<<l) <= n; ++i)
            val[l][i] = max(val[l-1][i], val[l-1][i + (1<<(l-1))]);
}
inline int query(int l, int r){ // [l, r]
    int d = 31 - __builtin_clz(r - l + 1);
    return max(val[d][l], val[d][r - (1<<d) + 1]);
}
```

## ZCK Segment Tree

```cpp
struct ZCK{
    #define ll long long
    int n;
    ll val[N<<1], tag[N];
    inline void init(int _n, ll v[]){
        n = _n;
        for(int i = 0; i < n; ++i) val[i + n] = v[i];
        for(int i = n - 1; i > 0; --i)
            val[i] = val[i<<1] = val[i<<1|1], tag[i] = 0;
    }
    inline void upd(int p, ll v, int h){
        val[p] += v<<h;
        if(p < n) tag[p] += v;
    }
    inline void push(int p){
        for(int h = 31 - __builtin_clz(n); h >= 0; --h){
            int i = p>>h;
            if(tag[i>>1] == 0) continue;
            upd(i, tag[i>>1], h);
            upd(i^1, tag[i>>1], h);
            tag[i>>1] = 0;
        }
    }
    inline void pull(int p){
        for(int h = 1; p > 1; ++h, p>>=1)
            val[p>>1] = val[p] + val[p^1] + (tag[p>>1]<<h);
    }
    inline void modify(int l, int r, ll v){
        int ll = l, rr = r, h = 0;
        push(l + n), push(r + n - 1);
        for(l += n, r += n; l < r; l >>= 1, r >>= 1, ++h){
            if(l&1) upd(l++, v, h);
            if(r&1) upd(--r, v, h);
        }
        pull(ll + n), pull(rr + n - 1);
    }
    inline ll query(int l, int r){
        ll ans = 0;
        push(l + n), push(r + n - 1);
        for(l += n, r += n; l < r; l >>= 1, r >>= 1){
            if(l&1) ans += val[l++];
            if(r&1) ans += val[--r];
        }
        return ans;
    }
}
```

## Treap
```cpp
int val[N], l[N], r[N], sz[N], cnt = 1;
unsigned pri[N];
int tag[N];
inline unsigned ran(){
    static int x = 131;
    return ++(x *= 0xdefaced);
}
inline int newNode(int v){
    val[cnt] = v;
    l[cnt] = r[cnt] = 0;
    sz[cnt] = 1;
    tag[cnt] = 0;
    pri[cnt] = ran();
    return cnt++;
}
inline int Sz(int u){
    return u ? sz[u] : 0;
}
inline void push(int u){
    // push down tag
    // do something
    // clear tag
}
inline void pull(int u){
    sz[u] = Sz(l[u]) + Sz(r[u]) + 1;
    // update something?
}
void split(int o, int &a, int &b, int k){
    push(o);
    if(!o){
        a = b = 0;
        return;
    }
    if(v[a] <= k){
        a = o;
        splitBySize(r[o], r[a], b, k);
        pull(a);
    } else {
        b = o;
        splitBySize(l[o], a, l[b], k);
        pull(b);
    }
}
void splitBySize(int o, int &a, int &b, int k){
    push(o);
    if(!o){
        a = b = 0;
        return;
    }
    if(Sz(l[o]) + 1 <= k){
        a = o;
        splitBySize(r[o], r[a], b, k - Sz(l[o]) - 1);
        pull(a);
    } else {
        b = o;
        splitBySize(l[o], a, l[b], k);
        pull(b);
    }
}
int merge(int a, int b){
    push(a), push(b);
    if(!a or !b) return a ? a : b;
    if(pri[a] < pri[b]){
        r[a] = merge(r[a], b);
        pull(a);
        return a;
    }
    l[b] = merge(a, l[b]);
    pull(b);
    return b;
}
void inorder_traverse(int u){
    push(u);
    if(l[u]) inorder_traverse(l[u]);
    // cout << u << ' ';
    if(r[u]) inorder_traverse(r[u]);
}
```

# 圖論
---

## Disjoint Sets (DSU)

## Dijkstra

## Floyd-Warshall

## Doubling LCA

## Heavy-Light Decomposition

## Centroid Decomposition

## 樹壓平 O(1) LCA

## SCC

## BCC

## Bipartite Graph Matching


# 計算幾何
---

## Convex Hull

## Segment Intersection

## LawFung Sort

## Smallest Enclosing Circle


# 數學
---

## Gauss Elimination

## Modular Inverse

## FFT & NTT

## Euler's Totient Function


# 其他
---

## Mo's Algorithm
