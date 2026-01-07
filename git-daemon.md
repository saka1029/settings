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
GIT_DAEMON_OPTIONS="--eport-all --enable=receive-pack"
```

最後に以下を実行して完了です。

```
update-rc.d git-daemon defaults
```

## debian openmediavaultの場合

***!!! 以下はうまくいかない !!!***

```
sudo apt install git-daemon-sysvinit
```

`/etc/default/git-daemon`を編集する。

```
# Defaults for git-daemon initscript 
# sourced by /etc/init.d/git-daemon 
# installed at /etc/default/git-daemon by the maintainer scripts 
 
# 
# This is a POSIX shell fragment 
# 
 
GIT_DAEMON_ENABLE=true
GIT_DAEMON_USER=gitdaemon 
GIT_DAEMON_BASE_PATH=/srv/dev-disk-by-uuid-B45EDCD75EDC9388/git
GIT_DAEMON_DIRECTORY=/srv/dev-disk-by-uuid-B45EDCD75EDC9388/git
 
# Additional options that are passed to the Daemon. 
GIT_DAEMON_OPTIONS="--eport-all --enable=receive-pack"
```

`git-daemon-run`をインストールする。

```
sudo apt install git-gaemon-run
```
`/etc/sv/git-daemon/run`を編集する。
`--export-all`は各レポジトリに`git-daemon-export-ok`をtouchしなくてもよいようにする。

```
#!/bin/sh 
exec 2>&1 
echo 'git-daemon starting.' 
exec chpst -ugitdaemon \ 
  "$(git --exec-path)"/git-daemon --verbose --reuseaddr \ 
    --export-all \ 
    --base-path=/srv/dev-disk-by-uuid-B45EDCD75EDC9388 /srv/dev-disk-by-uuid-B45EDCD75EDC9388/git 
#   --base-path=/var/lib /var/lib/git
```
