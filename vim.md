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

## コマンド名 | 取得できる情報

expand("%") | ディレクトリ/名前(現在のディレクトリを基準)
expand('%:p") | フルパス
expand('%:p:h") | ディレクトリ
expand('%:t") | ファイル名
expand('%:r") | 拡張子なしのファイル名
expand('%:e") | 拡張子

## vimクリップボード

ヤンク(y)コマンドでクリップボードに貼り付けるにはvim-gtkが必要。
```
sudo apt install vim-gtk
```
or
```
sudo apt install vim-gtk3
```

.vimrcに以下を記述する。
```
set clipboard=unnamedplus
```

## vim ESCでIMEを抜ける

.vimrcに以下を追記。

```
augroup VIMRC
    if has('unix') " インサート・モードを抜けた時は必ず日本語 OFF
        autocmd InsertLeave,CmdwinLeave * call system('fcitx-remote -c')
    endif
augroup END
```

「autocmd」の行だけでもよい。
「has(x)」の引数は'win64`でwindowsとなる。

fcitx-remoteのオプションは以下のとおり。

```
Usage: fcitx-remote [OPTION]
	-c		inactivate input method
	-o		activate input method
	-r		reload fcitx config
	-t,-T		switch Active/Inactive
	-e		Ask fcitx to exit
	-a		print fcitx's dbus address
	-m <imname>	print corresponding addon name for im
	-s <imname>	switch to the input method uniquely identified by <imname>
	[no option]	display fcitx state, 0 for close, 1 for inactive, 2 for acitve
	-h		display this help and exit
```
