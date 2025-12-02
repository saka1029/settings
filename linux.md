# linux

## CapsLockキーとCtrlキーを入れ替え
```
gsettings set org.gnome.desktop.input-sources xkb-options "['ctrl:nocaps']"
```
CapsLockキーの設定を初期状態にリセットする
```
gsettings reset org.gnome.desktop.input-sources xkb-options
```

## フォルダ名英語化

```
LANG=C xdg-user-dirs-gtk-update
```
