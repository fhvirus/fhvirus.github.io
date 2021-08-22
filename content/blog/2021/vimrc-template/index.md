---
title: "Vimrc & Template"
date: 2021-02-06T22:16:44+08:00
description: "FHVirus 的 .vimrc 跟模板"
tags: 
  - 模板
images:
  - cover.jpg
katex: false
---

更新了一下所以就重新放的一篇。

<!--more-->

## .vimrc

大概功能：

- 蛋餅幫我裝的 `vim-plug`，因為我不會用，所以裡面空空如也。
- 設定 colorscheme ，顯示選項之類的。
- 快捷鍵：刷入模板，編譯，複製整份扣的等。
- **鎖住方向鍵並在你用了的時候嘲諷你**。

```vim
set encoding=utf-8

set t_Co=256
syntax on
colorscheme monokai

call plug#begin('~/.vim/plugged')
Plug 'https://github.com/gi1242/vim-tex-syntax.git'
Plug 'joker1007/vim-markdown-quote-syntax'
Plug 'cespare/vim-toml'
Plug 'godlygeek/tabular'
Plug 'vim-latex/vim-latex'
call plug#end()
" Plug 'plasticboy/vim-markdown'
" Plug 'gabrielelana/vim-markdown'

let t:is_transparent = 0
function! Toggle_transparent_background()
  if t:is_transparent == 0
    hi Normal guibg=NONE ctermbg=black
    let t:is_transparent = 1
  else
    hi Normal guibg=NONE ctermbg=NONE
    let t:is_transparent = 0
  endif
endfunction
nnoremap <C-x><C-t> :call Toggle_transparent_background()<CR>

set number
set relativenumber
set cindent
set cursorline
set ruler
set tabstop=2
set shiftwidth=2
set backspace=2
set showcmd
set hlsearch
set scrolloff=3
set laststatus=2
set nowrap
filetype indent on

" cpp settings
inoremap {<CR> {<CR>}<ESC>O
map<F3> :%d<CR>:r ~/OWO/template.cpp<CR>kJ21zF24G
map<F4> :tabe ~/OWO/in.in<CR>
map<F5> :%d<CR>"+P:w<CR><F8>
map<F7> :w<CR>:!g++ "%" -o ~/OWO/run -std=c++17 -Wfatal-errors -DOWO -fsanitize=undefined<CR>
map<F8> :w<CR>:!echo "\t\tinput\n" && cat ~/OWO/in.in && echo "\n\t\toutput\n" && ~/OWO/run < ~/OWO/in.in<CR>
map<F9> :%y+<CR>
map<C-l> :w<CR>:!g++ "%" -o ~/OWO/run<CR>
map<F2> :w<CR>:!g++ "%" grader.cpp -o ~/OWO/run -std=c++17 -Wfatal-errors -DOWO -fsanitize=undefined<CR>
map<C-p> I//<ESC>

" LaTeX settings
map<F6> :w<CR>:!xelatex "%" && open "%:r".pdf -a Preview && open -a terminal<CR><CR>

" Disabling settings
nnoremap <Up> :echo "No up for you!"<CR>
vnoremap <Up> :<C-u>echo "No up for you!"<CR>
inoremap <Up> <C-o>:echo "No up for you!"<CR>
nnoremap <Down> :echo "No down for you!"<CR>
vnoremap <Down> :<C-u>echo "No down for you!"<CR>
inoremap <Down> <C-o>:echo "No down for you!"<CR>
nnoremap <Left> :echo "No left for you!"<CR>
vnoremap <Left> :<C-u>echo "No left for you!"<CR>
inoremap <Left> <C-o>:echo "No left for you!"<CR>
nnoremap <Right> :echo "No right for you!"<CR>
vnoremap <Right> :<C-u>echo "No right for you!"<CR>
inoremap <Right> <C-o>:echo "No right for you!"<CR>

nmap <C-e> :call <SID>SynStack()<CR>
function! <SID>SynStack()
    if !exists("*synstack")
        return
    endif
    echo map(synstack(line('.'), col('.')), 'synIDattr(v:val, "name")')
endfunc
```



## 模板

也是更新了所以也放了新的。功能：

- 一些我自己習慣的 macro (`FOR`, `FOO`, `OOF`...) 。
- 一些抄蛋餅的 debug code （蛋餅的模板真香）。
- 方便打的輸入輸出（`RI`, `PL`）。
- 打 CF 專用的偷吃步變數宣告。

壓過字元數的版本，整個才 40 行左右，debug 的部份也不到 1kB 真的超棒。

```cpp
// Knapsack DP is harder than FFT.
#ifndef OWO
#include<bits/stdc++.h>
#endif
#include<bits/extc++.h>
using namespace std;
using namespace __gnu_pbds;
typedef int64_t ll;
typedef pair<int,int> pii;
#define ff first
#define ss second
#define pb emplace_back
#define FOR(i,n) for(int i=0;i<(n);++i)
#define FOO(i,a,b) for(int i=(a);i<=int(b);++i)
#define OOF(i,a,b) for(int i=(a);i>=int(b);--i)
#define AI(x) (x).begin(),(x).end()
template<class I>bool chmax(I&a,I b){return a<b?(a=b,true):false;}
template<class I>bool chmin(I&a,I b){return b<a?(a=b,true):false;}
template<class V>void lisan(V&v){sort(AI(v));v.erase(unique(AI(v)),v.end());}
template<class V,class T>int lspos(const V&v,T x){return lower_bound(AI(v),x)-v.begin();}
template<class...T>void RI(T&...t){(...,(cin>>t));}
template<class...T>void PL(T...t){int c=sizeof...(T);if(!c){cout<<'\n';return;}(...,(cout<<t<<(--c?' ':'\n')));}
constexpr inline ll cdiv(ll x,ll m){return x/m+(x%m?(x<0)^(m>0):0);}
constexpr inline ll mpow(ll x,ll e,ll m){ll r=1;for(x%=m;e;e/=2,x=x*x%m)if(e&1)r=r*x%m;return r;}
#ifdef OWO
#define safe cerr<<"\033[1;32m"<<__PRETTY_FUNCTION__<<" - "<<__LINE__<<" JIZZ\033[0m\n"
#define debug(args...) SDF(#args, args)
#define OIU(args...) ostream& operator<<(ostream&O,args)
#define LKJ(S,B,E,F) template<class...T>OIU(S<T...>s){O<<B;int c=0;for(auto i:s)O<<(c++?", ":"")<<F;return O<<E;}
LKJ(vector,'[',']',i)LKJ(deque,'[',']',i)LKJ(set,'{','}',i)LKJ(multiset,'{','}',i)LKJ(unordered_set,'{','}',i)LKJ(map,'{','}',i.ff<<':'<<i.ss)LKJ(unordered_map,'{','}',i.ff<<':'<<i.ss)
template<class...T>OIU(pair<T...>p){return O<<'('<<p.ff<<','<<p.ss<<')';}
template<class T,size_t N>OIU(array<T,N>a){return O<<vector<T>(AI(a));}
template<class...T>OIU(tuple<T...>t){return O<<'(',apply([&O](T...s){int c=0;(...,(O<<(c++?", ":"")<<s));},t),O<<')';}
template<class...T>void SDF(const char* s,T...a){int c=sizeof...(T);if(!c){cerr<<"\033[1;32mvoid\033[0m\n";return;}(cerr<<"\033[1;32m("<<s<<") = (",...,(cerr<<a<<(--c?", ":")\033[0m\n")));}
#else
#pragma GCC optimize("Ofast")
#define safe ((void)0)
#define debug(...) ((void)0)
#endif
constexpr ll inf = 1e9, INF = 4e18;
const int N = 2e5 + 225, M = 1e9 + 7;
int t, n, a[N];

int32_t main(){
	ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	return 0;
}
```

很令人傷心的，上面的 `class...T` 似乎被 Chroma 當成錯誤了。被分錯類的我也沒辦法救了，只好忍受他這樣醜醜的。
