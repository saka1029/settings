# tmux
## インストール

```
sudo apt install tmux -y
```

## 画面分割
* 左右: Ctrl-b %
* 上下: Ctrl-b "

## マウス有効化

~/.tmux.conf

```
set-option -g mouse on
```

## 起動時にsshd実行

`.bashrc`に以下の行を追加する。

```
sshd
```
