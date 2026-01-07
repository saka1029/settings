# gitプロトコル

ネットワーク越しに使いたい場合はgitプロトコルで運用するのが楽です。
ただしセキュリティ的に問題があるのでLAN内だけの使用に限定すべきでしょう。
なおすでに前回の作業を行い、サーバ側クライアント側共に
localプロトコルでは 使用可能であることを前提とします。

まずはサーバ側で以下を実行します。
```
server$ cd /opt/git
server$ git init --bare --shared test.git             # repository初期化
server$ cd test.git　　　　　　　　　　　　　　　　　　　　　# repositoryに入る
server$ touch git-daemon-export-ok                    # export可とする
server$ git config --global --add safe.directory .    # 参照可とする
server$ git config daemon.receivepack true            # pushで更新可とする
server$ sudo chown -R gitdaemon /opt/git              # 所有者変更
server$ git daemon --base-path=/opt/git /opt/git      # git-daemon起動
```

続いてクライアント側で以下を実行します。

```
client$ cd
client$ mkdir git
client$ cd git
client$ git clone git://<サーバの名前やIPアドレス>/test.git
```

これで"test"ディレクトリができており、これが空のリポジトリになっているので、
ここにファイルを登録するなどすることになります。
ネット越しなのに認証なしでアクセスできる。

なお、このままだとサーバ側の"git daemon"を止めると
gitプロトコルでのアクセスができなくなります。
これはサービス化する方法が用意されています。

まず以下で起動スクリプトをインストールします。

```
apt install git-daemon-sysvinit
```

スクリプト"/etc/init.d/git-daemon"ができているので
少なくとも以下のような設定になるよう修正します。

```
GIT_DAEMON_USER=gitdaemon
GIT_DAEMON_ENABLE=true
GIT_DAEMON_BASE_PATH=/opt/git
GIT_DAEMON_DIRECTORY=/opt/git
```

最後に以下を実行して完了です。

```
update-rc.d git-daemon defaults
```
