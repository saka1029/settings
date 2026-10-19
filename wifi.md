# wifi

## パスワード (Speed Wi-Fi DOCK 5G 01)
パスワードに使用できる文字は、数字（0 ～ 9）、大文字アルファベット
（A ～ Z）、小文字アルファベット（a ～ z）で、半角 12 文字以上、最大半
角 63 文字まで有効です

## 事前準備

nmcliコマンドおよびnmtuiコマンドが見つからない場合は、NetworkManagerを以下のコマンドでインストールする。

```
sudo apt-get install network-manager
```

## Wi-Fiをオンにする

```
nmcli radio wifi on
```

## nmtuiを起動する

```
nmtui
```
<img width="388" height="321" alt="image" src="https://github.com/user-attachments/assets/27ea5c52-14fe-420b-af0d-a656f9efd641" />

「接続をアクティベートする」を選択する

SSIDから接続先を選んで接続する

「終了」を選択する
