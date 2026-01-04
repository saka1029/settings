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

## 日付書式

2025-12-11 木曜日 12:41

```
%Y-%m-%e %A %H:%M
```

## Javaバージョン切り替え

```
sudo update-alternatives --config java
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

## 輝度調整できない場合（Let's Note + Linux Mintのみ？）

Grubの設定ファイルを開く。

```
sudo vi /etc/default/grub
```

10行目あたりに「GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"」と書かれた行があるので、
以下のように「 acpi_backlight=video acpi_osi=!!」を付け加えて保存する。

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_backlight=video acpi_osi=!!"
```

Grubを更新する。

```
sudo update-grub
```

再起動する。

## コマンドによる画面回転

コマンドで回転作業を行う場合は、

```
xrandr -o right
```
```
xrandr -o left
```
```
xrandr -o inverted
```
```
xrandr -o normal
```

## mDNS有効化

avahi-daemonをインストールする。

```
sudo apt update
sudo apt install avahi-daemon
sudo systemctl start avahi-daemon
sudo systemctl enable avahi-daemon
```

## ホストネームの変更

ホストネームを変更する場合は下記手順で変更できる

```
sudo hostnamectl set-hostname <新しいホストネーム>
sudo vim /etc/hosts # 127.0.0.1 <新しいホストネーム> に変更
```

## mDNSの許可

そのままだと、mDNSの問い合わせがファイアウォールに弾かれてしまうため、
mDNSで利用するUDPの5353番ポートを開放する。
ここでは、ufwの有効化方法については記載しない。

```
sudo ufw allow mdns
```
