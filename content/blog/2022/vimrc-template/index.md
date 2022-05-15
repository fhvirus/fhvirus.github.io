---
title: ".vimrc"
date: 2022-05-15T17:08:49+08:00
description: "FHVirus 的 .vimrc"
tags:
  - 模板
katex: true
---

~~看雞塊的[這篇](https://rk42745417.github.io/vimrc/)看的心癢癢，更新了兩下所以就重新放的一篇。~~

因為我的 `.vimrc` 又進化了，所以再放一次。

<!--more-->

---

我現在的 `.vimrc` 長這樣：

```vim
source ~/.vim/options.vim
source ~/.vim/comment.vim
source ~/.vim/keymaps.vim
source ~/.vim/togglet.vim
source ~/.vim/plugins.vim
```

我把正常會寫在 `.vimrc` 裡面的東西分裝到 `.vim` 底下各個檔案裡面了。所有檔案在 [這裡](https://github.com/fhvirus/vimrc) 找的到，底下也會一一解說。
如果有初學者想要用的話也不要害怕，把檔案貼一貼路徑改一改就對了 ><

大概功能：

- `options.vim` 裡面裝著普通的設定（ `set blablabla` ）。
- `comment.vim` 是自製的註解工具。
- `keymaps.vim` 是大部分的快捷鍵。
- `togglet.vim` 是切換透明與不透明背景的工具。
- `plugins.vim` 裝的 `vim-plug` 相關的東西以及針對插件的設定。
- 對於特定檔案類型才有的快捷建放在 `ftplugin/<lang>.vim` 裡面。

---

### `options.vim`

```vim
" se enc=utf-8 scripte=utf-8 fenc=utf-8
set encoding=utf-8
scriptencoding=utf-8
set fileencoding=utf-8

" se t_Co=256 tgc | sy on | colo monokai
set t_Co=256
set termguicolors
syntax on
colorscheme monokai

" se nu ru rnu cin cul sc so=4 ts=2 sw=2 bs=2 ls=2 bo=a
set number
set ruler
set relativenumber
set cindent
set cursorline
set showcmd
set scrolloff=4
set tabstop=2
set shiftwidth=2
set backspace=2
set laststatus=2
set belloff=all

" se hls is ic scs
set hlsearch
set incsearch
set ignorecase
set smartcase

" se nowrap | filet plugin indent on
set nowrap
filetype plugin indent on
```

#### 功能簡介

想知道每個在幹嘛的話請自己在 vim 裡面打 `:help <option>` 。

1. 基本的設定編碼。不確定是怎麼運作的所以不敢拿掉。
2. 顏色相關的東西。我自己是用 [這個](https://github.com/crusoexia/vim-monokai) 然後把他的 `monokai.vim` 直接貼到 `~/.vim/colors` 底下。
3. 正常的輔助功能設定，比賽的時候會打上面縮減的那段。
4. 搜尋相關的東西，還蠻實用的，從 [這裡](https://morioprog.github.io/2020/06/vim_memo/) 抄來的。
5. 雜項。要開著 `filetype plugin on` 才可以用 `~/.vim/ftplugin/` 裡面的東西。

#### 精簡版

```vim
sy on | colo monokai | filet plugin indent on
se enc=utf-8 scripte=utf-8 fenc=utf-8 t_Co=256 tgc nu ru rnu cin cul sc so=4 ts=2 sw=2 bs=2 ls=2 bo=a hls is ic scs nowrap
```

---

### `comment.vim`

```vim
" Commenting blocks of code.
" The following line can also be used for .cpp files:
" autocmd FileType cpp noremap <buffer> <silent> <C-P> :<C-B>silent! <C-E>s#^\(\s*\)#\1// <CR> :<C-B>silent! <C-E>s#^\(\s*\)// //\s*#\1<CR>
augroup commenting_blocks_of_code
  autocmd!
  autocmd FileType c,cpp,java,scala let b:comment_leader = '//'
  autocmd FileType sh,ruby,python   let b:comment_leader = '#'
  autocmd FileType tex              let b:comment_leader = '%'
  autocmd FileType vim              let b:comment_leader = '"'
augroup END
nnoremap <silent> <C-P> :<C-B>silent <C-E>s,^\(\s*\),\1<C-R>=b:comment_leader<CR> ,e <bar>
		\s,^\(\s*\)<C-R>=b:comment_leader<CR> <C-R>=b:comment_leader<CR>\s*,\1,e<CR>:nohlsearch<CR>
vnoremap <silent> <C-P> :<C-B>silent <C-E>g/^/ s,\(\s*\),\1<C-R>=b:comment_leader<CR> ,e <bar>
		\s,\(\s*\)<C-R>=b:comment_leader<CR> <C-R>=b:comment_leader<CR>\s*,\1,e<CR>:nohlsearch<CR>
```

從 [這裡](https://stackoverflow.com/a/1676672) 改的，基本上就是用 regex 來做取代而已，長的有點醜，不知道要怎麼讓他變漂亮。

---

### `keymaps.vim`

```vim
inoremap {<CR> {<CR>}<ESC>O
noremap ; :
nmap <F5> :%d<CR>"+P:w<CR>
nmap <F9> :%y+<CR>
nnoremap <silent> <ESC><ESC> :<C-U>set nohlsearch!<CR>
nnoremap ␛r :so ~/.vimrc<CR>

" NERDTree mappings
nnoremap ␛n :NERDTreeFocus<CR>
nnoremap ␛t :NERDTreeToggle<CR>
nnoremap ␛f :NERDTreeFind<CR>
```

少少的，除了自動大括號和懶人冒號之外沒什麼特別的。
上面的怪怪 `esc` 是因為 alt 鍵要用怪怪的方式 map （先按下 `CTRL-V` 然後再按需要的 keybind ）。

---

### `togglet.vim`

```vim
" Toggle transparent background
let g:is_transparent = 1
function! Toggle_transparent_background()
  if g:is_transparent == 0
    " hi Normal guibg=NONE ctermbg=234
		set tgc
		colo monokai
    let g:is_transparent = 1
  else
    " hi Normal guibg=NONE ctermbg=NONE
		set tgc!
		colo ron
    let g:is_transparent = 0
  endif
endfunction
nnoremap ␛k :call Toggle_transparent_background()<CR>
```

切換透明背景用的。不知道為什麼有 `tgc` 的時候就不能用透明背景了，反正可以用就好。
註解掉的那兩行在沒有 `set tgc` 的情況下也是可以用的。

---

### `plugins.vim`

```vim
call plug#begin('~/.vim/plugged')
Plug 'cespare/vim-toml'
Plug 'pangloss/vim-javascript'
Plug 'joker1007/vim-markdown-quote-syntax'
Plug 'vim-latex/vim-latex'
Plug 'gi1242/vim-tex-syntax'
Plug 'godlygeek/tabular'
Plug 'rlue/vim-barbaric'
Plug 'fhvirus/learn-hjkl'
Plug 'preservim/nerdtree'
call plug#end()

" for barbaric
set ttimeoutlen=0
```

- `cespare/vim-toml`、`pangloss/vim-javascript`、`joker1007/vim-markdown-quote-syntax` 是一些擴充的 syntax highlight 。
- `vim-latex/vim-latex`、`gi1242/vim-tex-syntax` 是 $\TeX$ 的輔助功能。
- `godlygeek/tabular` 是自動對齊的東西，還蠻好用的。
- `rlue/vim-barbaric` 在 normal/insert mode 切換的時候自動切換中英文，省的一直手動切。
- `fhvirus/learn-hjkl` ZCK 提供的強迫學習 hjkl 用工具。
- `preservim/nerdtree` 側邊檔案欄。我還不太會用。

---

### `ftplugin/cpp.vim`

```vim
nmap <buffer> <F3> :%d<CR>:r ~/OWO/templates/template.cpp<CR>kJ18zF22G
nmap <buffer> <F4> :tabe ~/OWO/in.in<CR>
nmap <buffer> <F6> :%d<CR>:r ~/OWO/templates/minimum.cpp<CR>kJ6G
nmap <buffer> <F7> :w<CR>:!g++ "%" -o ~/OWO/run -std=c++17 -Wall -DOWO -fsanitize=undefined<CR>
nmap <buffer> <F8> :w<CR>:!echo "\t\tinput\n" && cat ~/OWO/in.in && echo "\n\t\toutput\n" && ~/OWO/run < ~/OWO/in.in<CR>

" clang-format mappings
map <C-K> :py3f ~/.vim/clang-format.py<CR>
imap <C-K> <C-O>:py3f ~/.vim/clang-format.py<CR>

" Load template
command -buffer -nargs=1 Load :r ~/OWO/templates/<args>.cpp

" Hash selected range (from kactl)
ca Hash w !cpp -dD -P -fpreprocessed \| tr -d '[:space:]' \| md5sum \| cut -c-6
```

一些 `C++` 專用的快捷建。

1. 刷 default code ，快速執行、編譯。
2. `clang-format` 真的是個好東西。可以自動把醜醜的 code 變得很漂亮，強烈建議裝（說明在 [這裡](https://clang.llvm.org/docs/ClangFormat.html#vim-integration) ）。
3. 讀模板。
4. 我不確定會不會用到但是先放著了。

---

大概就這樣吧。
