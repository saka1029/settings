# VNCを自動起動に設定する
TigerVNCサーバーを自動起動する設定 (systemd)
この設定は、特定のユーザー（例えば ubuntu ユーザー）のVNCセッションを自動起動させるためのものです。

(Orange pi zero 3 w で設定確認)

## VNCサーバーの停止
まず、現在起動しているVNCサーバーを停止します。
```
vncserver -kill :1
```
(もし :1 以外のディスプレイ番号で起動しているVNCサーバーがあれば、そちらも停止してください。ps aux | grep Xtigervnc で確認できます。)

## systemdサービスファイルを作成する

自動起動設定のために、新しいsystemdサービスファイルを作成します。ファイル名は vncserver@.service のように、ユーザーごとに柔軟に対応できるようにテンプレート形式にします。

サービスファイルの作成: 以下のコマンドで、テキストエディタでサービスファイルを開きます。
```
sudo vi /etc/systemd/system/vncserver@.service
```
サービスファイルの内容の記述: 開いたファイルに、以下の内容を記述します。

開いたファイルに、以下の内容を記述します。 
```
[Unit]
Description=TigerVNC Remote Desktop Service
After=network.target

[Service]
Type=forking
User=saka1029
# Group=saka1029 # オプション: ユーザーと同じグループで実行する場合
WorkingDirectory=/home/saka1029h

PIDFile=/home/saka1029/.vnc/%H:%i.pid
ExecStartPre=-/usr/bin/vncserver -kill :%i
# -localhost no オプションは無効かもしれない。
# コマンドラインで`vncserver --help`を実行して確認する。
ExecStart=/usr/bin/vncserver -localhost no :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

    Description: サービスの説明。
    After=network.target: ネットワークサービスが起動した後にVNCサーバーを起動します。
    Type=forking: VNCサーバーがフォークしてバックグラウンドで実行されることを示します。
    User: VNCセッションを実行するユーザー名。ここにあなたのユーザー名を入れてください。
    WorkingDirectory: ユーザーのホームディレクトリ。
    PIDFile: プロセスIDが保存されるファイルのパス。
    ExecStartPre: VNCサーバー起動前に、同じディスプレイ番号の既存セッションを強制終了します。
    ExecStart: VNCサーバーを起動するコマンド。
        -localhost no: 外部からの接続を許可する重要なオプションです。
        :%i: vncserver@.service の @ の後に指定されるインスタンス番号（ディスプレイ番号）に置き換わります。
    ExecStop: VNCサーバーを停止するコマンド。
    WantedBy=multi-user.target: マルチユーザーモード（CUIログイン可能な状態）でサービスが有効になることを示します。

ファイルを保存して終了します。

## systemdにサービスを登録し、有効化する
サービスファイルを作成したら、systemdにそのサービスを認識させ、起動時に有効にします。

systemd設定をリロード: 新しいサービスファイルを読み込ませるために、systemdの設定をリロードします。

```
sudo systemctl daemon-reload
```

VNCサービスを有効化:

VNCサーバーを自動起動するように有効化します。@1 の 1 は、起動したいディスプレイ番号です。通常は :1 なので 1 を指定します。
```
sudo systemctl enable vncserver@1.service
```
これにより、/etc/systemd/system/multi-user.target.wants/vncserver@1.service へのシンボリックリンクが作成されます。
## VNCサービスを開始する
すぐにVNCサーバーを起動したい場合は、以下のコマンドを実行します。
システムを再起動するまで待つ必要はありません。
```
sudo systemctl start vncserver@1.service
```

## 起動状況の確認
VNCサーバーが正常に起動しているか、ss コマンドと systemctl status コマンドで確認します。

ポートのリッスン状況確認:
```
sudo ss -tulnp | grep 5901 0.0.0.0:5901 と表示されていればOKです。
```
サービスの状態確認:
```
systemctl status vncserver@1.service Active: active (running) と表示されていれば、サービスは正常に動作しています。
```
エラーメッセージが出ていないかも確認してください。

## VPSを再起動して確認 (任意だが推奨)

すべての設定が正しく機能するかを確認するために、VPSを一度再起動してみることをお勧めします。

```
sudo reboot
```
再起動後、VNCクライアントから自動的に起動したVNCセッションに接続できるか試してください。

これで、VPSが起動するたびにVNCサーバーが自動的に起動し、VNCアクセスが可能になるはずです。
