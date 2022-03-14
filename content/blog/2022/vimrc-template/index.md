---
title: "Vimrc & Template"
date: 2022-03-14T21:20:51+08:00
description: "FHVirus 的 .vimrc 跟模板"
tags:
  - 模板
images:
  - cover.jpg
katex: false
---

看雞塊的[這篇](https://rk42745417.github.io/vimrc/)看的心癢癢，更新了兩下所以就重新放的一篇。

<!--more-->

## .vimrc

大概功能：

- `vim-plug` 裡面的插件我懶得介紹，自己去 GitHub 找。
- 設定 colorscheme ，顯示選項之類的正常 vimrc。
- 快捷鍵：刷入模板，編譯，複製整份扣的等。裡面快捷建可以看到有一個資料夾叫 `OWO`，那是我平常打扣用的專門資料夾，裡面存有寫好可以直接 `:Load` 的模板。連結在[這裡](https://github.com/fhvirus/OWO)喔。
- **鎖住方向鍵並在你用了的時候嘲諷你**。
- 自動排版格式用的 `ClangFormat` （超推）。
- 可以一邊打扣一邊看推直播的透明 terminal 設定。值得一題的是要先去把 terminal 本身的背景換掉才可以喔。

```vim
set encoding=utf-8
scriptencoding=utf-8
set fileencoding=utf-8

set t_Co=256
syntax on

call plug#begin('~/.vim/plugged')
Plug 'https://github.com/gi1242/vim-tex-syntax.git'
Plug 'joker1007/vim-markdown-quote-syntax'
Plug 'cespare/vim-toml'
Plug 'godlygeek/tabular'
Plug 'vim-latex/vim-latex'
Plug 'rhysd/vim-clang-format'
Plug 'pangloss/vim-javascript'
Plug 'rlue/vim-barbaric'
call plug#end()

" Short version:
" se nu ru rnu cin cul sc so=4 ts=2 sw=2 bs=2 ls=2 bo=all
set number
set ruler
set relativenumber
set cindent
set cursorline
set showcmd
set scrolloff=3
set tabstop=2
set shiftwidth=2
set backspace=2
set laststatus=2
set belloff=all

" Search settings
set hlsearch
set incsearch
set ignorecase
set smartcase
" <ESC><ESC> : toggle highlight
nnoremap <silent><Esc><Esc> :<C-u>set nohlsearch!<CR>

set nowrap
filetype indent on

" key mapping
inoremap {<CR> {<CR>}<ESC>O
map<F9> :%y+<CR>

" cpp settings
autocmd filetype cpp map <F3> :%d<CR>:r ~/OWO/templates/template.cpp<CR>kJ18zF21G
autocmd filetype cpp map <F4> :tabe ~/OWO/in.in<CR>
autocmd filetype cpp map <F5> :%d<CR>"+P:w<CR>
autocmd filetype in  map <F5> :%d<CR>"+P:w<CR><F8>
autocmd filetype cpp map <F6> :%d<CR>:r ~/OWO/templates/minimum.cpp<CR>kJ5G
autocmd filetype cpp map <F7> :w<CR>:!g++ "%" -o ~/OWO/run -std=c++17 -Wfatal-errors -DOWO -fsanitize=undefined<CR>
autocmd filetype cpp map <F8> :!echo "\t\tinput\n" && cat ~/OWO/in.in && echo "\n\t\toutput\n" && ~/OWO/run < ~/OWO/in.in<CR>
autocmd filetype in  map <F8> :w<CR>:!echo "\t\tinput\n" && cat ~/OWO/in.in && echo "\n\t\toutput\n" && ~/OWO/run < ~/OWO/in.in<CR>
autocmd filetype cpp nnoremap <C-C> :s/^\(\s*\)/\1\/\/<CR> :s/^\(\s*\)\/\/\/\//\1<CR> $

" Load template
command -nargs=1 Load :r ~/OWO/templates/<args>.cpp

" LaTeX compile
autocmd filetype tex map <F7> :w<CR>:!xelatex -shell-escape "%"<CR>:!xdg-open "%:r".pdf<CR><CR>

" Disable arrowkeys
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

" For barbaric
set ttimeoutlen=0

" Clang-format settings
let g:clang_format#style_options = {
						\ "IndentWidth": 2,
						\ "ColumnLimit": 80,
						\ "AlignTrailingComments": "true",
            \ "AccessModifierOffset" : -4,
            \ "AllowShortIfStatementsOnASingleLine" : "true",
            \ "AlwaysBreakTemplateDeclarations" : "true",
            \ "Standard" : "C++11"}

" map to <Leader>kl in C++ code
autocmd FileType c,cpp,objc nnoremap <buffer><C-K><C-L> :<C-u>ClangFormat<CR>
autocmd FileType c,cpp,objc vnoremap <buffer><Leader>kl :ClangFormat<CR>
" if you install vim-operator-user
autocmd FileType c,cpp,objc map <buffer><Leader>x <Plug>(operator-clang-format)
" Toggle auto formatting:
" nmap <Leader>C :ClangFormatAutoToggle<CR>

" Toggle transparent background
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
```

## 模板

再度更新。我現在越來越喜歡把東西寫的原汁原味清楚明瞭，所以拔掉了一些 `macro` 。
壓過字元數的版本，整個才 20 行左右，debug 的部份也不到 1kB 真的超棒。

```cpp
// Knapsack DP is harder than FFT.
#include<bits/stdc++.h>
using namespace std;
typedef long long ll; typedef pair<int,int> pii;
#define ff first
#define ss second
#define pb emplace_back
#define AI(x) begin(x),end(x)
#ifdef OWO
#define debug(args...) SDF(#args, args)
#define OIU(args...) ostream& operator<<(ostream&O,args)
#define LKJ(S,B,E,F) template<class...T>OIU(S<T...>s){O<<B;int c=0;for(auto i:s)O<<(c++?", ":"")<<F;return O<<E;}
LKJ(vector,'[',']',i)LKJ(deque,'[',']',i)LKJ(set,'{','}',i)LKJ(multiset,'{','}',i)LKJ(unordered_set,'{','}',i)LKJ(map,'{','}',i.ff<<':'<<i.ss)LKJ(unordered_map,'{','}',i.ff<<':'<<i.ss)
template<class...T>void SDF(const char* s,T...a){int c=sizeof...(T);if(!c){cerr<<"\033[1;32mvoid\033[0m\n";return;}(cerr<<"\033[1;32m("<<s<<") = (",...,(cerr<<a<<(--c?", ":")\033[0m\n")));}
template<class T,size_t N>OIU(array<T,N>a){return O<<vector<T>(AI(a));}template<class...T>OIU(pair<T...>p){return O<<'('<<p.ff<<','<<p.ss<<')';}template<class...T>OIU(tuple<T...>t){return O<<'(',apply([&O](T...s){int c=0;(...,(O<<(c++?", ":"")<<s));},t),O<<')';}
#else
#define debug(...) ((void)0)
#endif

signed main() {
	ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	return 0;
}
```

很令人傷心的，上面的 `class...T` 似乎被 Chroma 當成錯誤了。被分錯類的我也沒辦法救了，只好忍受他這樣醜醜的。
