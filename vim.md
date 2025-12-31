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

" yankでクリップボードに貼り付ける。(vm-gtk3が必要)
set clipboard=unnamedplus
" yankでクリップボードに貼り付ける。(windows)
set clipboard=unmamed,autoselect

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
" ディレクトリ編集でバナーを消す
let g:netrw_banner = 0
```

## ディレクトリ編集中にファイルを追加

```
%
```

## vimで現在編集中のテストファイルをテスト

```
mvn test -Dtest=%:t:r
```

`%:t:r`は編集中の拡張子を除くファイル名になる。
同一プロジェクト内に同じファイルが複数あるとうまくいかない。

フルパスなら`%`でよい。

```
!node %
```

## コマンド名 | 取得できる情報

* expand("%") | ディレクトリ/名前(現在のディレクトリを基準)
* expand('%:p") | フルパス
* expand('%:p:h") | ディレクトリ
* expand('%:t") | ファイル名
* expand('%:r") | 拡張子なしのファイル名
* expand('%:e") | 拡張子
