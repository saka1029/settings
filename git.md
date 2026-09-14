# git

## .gitconfig

```
[credential] 
    helper = store 
[user] 
    name = saka1029 
    email = saka1029@gmail.com 
[core] 
    quotepath = false
[init]
    defaultBranch = main
[pull]
	rebase = false
```

## mine, epub, miz, sozoku クローン
```
git clone git://minipc.local/mine.git
git clone git://minipc.local/epub.git
git clone git://minipc.local/miz.git
git clone git://minipc.local/sozoku.git
```

sshの場合

```
git clone saka1029@minipc.local:/srv/dev-disk-by-uuid-B45EDCD75EDC9388/git/mine.git
```
末尾を変えてepub, miz, sozoku

## 現在のリモートURL確認

```
git remote -v
```

## 新しいリモートURLに変更する(リポジトリ変更)

```
git remote set-url origin {new url}
```

## minipc ドライブパス

* D=/srv/dev-disk-by-uuid-B45EDCD75EDC9388/
* WDSSD4T=/srv/dev-disk-by-uuid-8C04A56804A5564C/
* WDBLUE=/srv/dev-disk-by-uuid-A676F8A876F87A7F/
* BUR2T=/srv/dev-disk-by-uuid-0A72FB3772FB25DB/
* SDCARD=/srv/dev-disk-by-uuid-DAE47F04E47EE265/
* BACKUP_MINIPC_D=/srv/dev-disk-by-uuid-5E10649B10647C41/BACKUP-MINIPC-D/

## MINIPCの$D/gitの下にbare repository 新規作成

### サーバー側
```
cd $D/git
git init --bare --shared NAME.git
cd NAME.git
git config --global --add safe.directory .
git config daemon.receivepack true            # pushで更新可とする
```

### クライアント側
```
cd git
git clone git://minipc.local/NAME.git 
```

## iplay60のgitリポジトリにアクセス

/etc/hostsにiplay60のエントリを追加する。
iplay60のIPアドレスはWifiルータで固定済である。

```
127.0.0.1	localhost
127.0.1.1	lifebook
#192.168.188.192	iplay60.local
192.168.188.192	iplay60

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

sshでログインする。

```
ssh iply60 -p 8022
```

クローンを作成する。

```
git clone ssh://iplay60:8022/~/repository/test
```

パスワード入力を省略したい場合はsshの公開鍵をiplay60の~/.ssh/authorized_keyに貼り付ける。
