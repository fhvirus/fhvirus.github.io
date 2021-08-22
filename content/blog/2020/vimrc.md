---
title: "FHVirus' vimrc"
date: 2020-11-14T22:16:45+08:00
description: "FHVirus 用的 .vimrc 。"
draft: true
---

因為剛剛自己耍笨覆蓋掉了導致必須手動重打，想說乾脆來寫一篇順便備份好了。

<!--more-->

## Vimrc

完全沒有裝 PlugIn（其實裝了 `vim-latex-live-preview` ，但是一直 `Failed to compile` 就當作沒裝吧）。

應該自己看就會懂了就不解釋囉。

```cpp
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

順便放上裡面的 cftemp.cpp。大部分都是抄 `darry140` 在 APIO 2018 官解裡面的東西。

```cpp {linenos=table}
//Knapsack DP is harder than FFT.
#pragma GCC optimize("Ofast")
#include<bits/stdc++.h>
using namespace std;
//types
typedef long long ll;
typedef pair<int,int> pii;
//input
bool SR(int &_x){return scanf("%d",&_x)==1;}bool SR(ll &_x){return scanf("%lld",&_x)==1;}
bool SR(double &_x){return scanf("%lf",&_x)==1;}bool SR(char *_x){return scanf("%s",_x)==1;}
bool RI(){return true;}
template<typename I,typename... T>bool RI(I &_x,T&... _tail){return SR(_x) && RI(_tail...);}
//output
void SP(const int _x){printf("%d",_x);}void SP(const ll _x){printf("%lld",_x);}
void SP(const double _x){printf("%.12lf",_x);}void SP(const char *_x){printf("%s",_x);}
void PL(){puts("");}
template<typename I,typename... T>void PL(const I _x,const T... _tail)
{SP(_x);if(sizeof...(_tail))putchar(' ');PL(_tail...);} 
//macro
#define SZ(x) ((int)(x).size())
#define ALL(x) (x).begin(),(x).end()
#define FOR(i,n) for(int i=0;i<(n);++i)
#define FOO(i,a,b) for(int i=(a);i<=int(b);++i)
#define OOF(i,a,b) for(int i=(a);i>=int(b);--i)
#define pb push_back
#define ff first
#define ss second
#define mkp make_pair
//debug
#ifdef OWO
template<typename A,typename B>
ostream& operator<<(ostream& _s, const pair<A,B> &_p){return _s<<'('<<_p.ff<<','<<_p.ss<<')';}
template<typename It>ostream& _OUTC(ostream& _s,It _b,It _e){
    _s<<'{';
    for(auto _it=_b;_it!=_e;++_it) _s<<(_it==_b?"":" ")<<*_it;
    _s<<'}';
    return _s;
}
template<typename A,typename B> 
ostream& operator <<(ostream&_s, const map<A,B> &_c){return _OUTC(_s,ALL(_c));} 
template<typename T> 
ostream& operator <<(ostream&_s, const set<T> &_c){return _OUTC(_s,ALL(_c));} 
template<typename T> 
ostream& operator <<(ostream&_s, const vector<T> &_c){return _OUTC(_s,ALL(_c));}
template<typename I>
void _DOING(const char *_s,I&& _x){cerr<<_s<<'='<<_x<<endl;}
template<typename I,typename... T>
void _DOING(const char *_s,I&& _x,T&&... _tail){
    int _c=0;
    static const char _bra[]="([{";
    static const char _ket[]=")]}";
    while(*_s!=',' || _c!=0){
        if(strchr(_bra,*_s)) _c++;
        if(strchr(_ket,*_s)) _c--;
        cerr<<*_s++;
    }
    cerr<<'='<<_x<<", ";
    _DOING(_s+1,_tail...);
}
#define debug(...) do{\
    fprintf(stderr,"%s:%d - ",__PRETTY_FUNCTION__,__LINE__);\
    _DOING(#__VA_ARGS__,__VA_ARGS__);\
}while(0)
#else
#define debug(...)
#endif
const int N = 1e5 + 225;
int t, n, a[N];

int32_t main(){
	ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
	return 0;
}
```
