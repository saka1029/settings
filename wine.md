# wine

## 日本語フォントのインストール

一度```winecfg```を起動する。

dataにある以下のファイルを

* msgothic.ttc
* msmincho.ttc

以下のディレクトリにコピーする。

```
~/.wine/drive_c/windows/Fonts
```
## 画面解像度の設定
```
winecfg
```

<img width="500" height="495" alt="image" src="https://github.com/user-attachments/assets/86b95e10-afae-423c-81e7-bf6795d5c201" />

## Paint Shop Pro 9インストール

ファイルサーバーから~/pspにインストールイメージをコピーする。

以下のコマンドでインストーラを起動する。

```
wine psp/Paint\ Shop\ Pro\ 9/autorun.exe
```

旧バージョンのインストールディレクトリを聞かれたら以下を入力する。

```
Z:\home\saka1029\psp\Paint Shop Pro 7
```

<img width="499" height="420" alt="image" src="https://github.com/user-attachments/assets/0a116e3d-8e80-4344-8477-7d2abf8e4023" />
