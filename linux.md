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

## HomeキーをBackSpaceに変更 (Linux mintのみ？)

```
sudo cp /usr/share/X11/xkb/symbols/pc /usr/share/X11/xkb/symbols/pc.bak
sudo vi /usr/share/X11/xkb/symbols/pc
```

```
  key <HOME> {[  Home    ]};
→
  key <HOME> {[  BackSpace    ]};
```
