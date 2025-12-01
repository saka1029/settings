# vim

## .vimrc

```
" .vimrc
syntax enable
colorscheme darkblue
set showmode
set showmatch
set shiftwidth=4
set tabstop=4
set expandtab
set nobackup
set noswapfile
set noundofile
set fileformats=unix,dos
" set fileformat=dos
set ignorecase
set belloff=all

set clipboard=unnamedplus

" CTRL-X are Cut
" vnoremap <C-X> "+x
" CTRL-C are Copy
" vnoremap <C-C> "+y
" CTRL-V are Paste
" map <C-V>       "+gP

" set guifont=MS_Gothic:h12

" Linux Mintの場合:
" set guifont=Noto\ Mono\ 12
" inoremap <silent> <C-[> <ESC>:call system('fcitx-remote -c')<CR>
" 挿入中にEscapeでIMEをoff
autocmd InsertLeave,CmdwinLeave * call system('fcitx-remote -c')
" autocmd insertLeave CmdwinLeave * call system('fcitx5-remote -c")
" autocmd InsertLeave,CmdwinLeave * call system('zenhan 0')
" 改行でコメント継続しない
" autocmd Filetype * set formatoptions-=r
" ハイフンで編集中ファイルのディレクトリに移動する
nnoremap - :<C-u>e %:h<Cr>
```

<img width="672" height="454" alt="image" src="https://github.com/user-attachments/assets/de79e235-1821-4bec-a091-a6340773519f" />
