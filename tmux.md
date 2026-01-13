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

事前にユーザ名をwhoamiで確認。
`passwd`でパスワードを設定しておく必要がある。
IPアドレスは`ifconfig`で確認する。

`.bashrc`に以下の行を追加する。

```
sshd
```

外部からは以下で接続する。

```
ssh -p 8022 u0_axxx@{IPアドレス}
