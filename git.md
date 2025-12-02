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
```
## ユーザー設定
git config --global user.name saka1029
git config --global user.email saka1029@gmail.com

## mine クローン
git clone saka1029@minipc.local:/srv/dev-disk-by-uuid-B45EDCD75EDC9388/git/mine.git

## epub クローン
git clone saka1029@minipc.local:/srv/dev-disk-by-uuid-B45EDCD75EDC9388/git/epub.git

## mizクローン
git clone saka1029@minipc.local:/srv/dev-disk-by-uuid-B45EDCD75EDC9388/git/miz.git

## 文字化け
git config --global core.quotepath false

## addとcommitを同時に

git commit -am "COMMENT"

## 現在のリモートURL確認

git remote -v

## 新しいリモートURLに変更する(リポジトリ変更)

git remote set-url origin {new url}

# minipc ドライブパス

D=/srv/dev-disk-by-uuid-B45EDCD75EDC9388/
WDSSD4T=/srv/dev-disk-by-uuid-8C04A56804A5564C/
WDBLUE=/srv/dev-disk-by-uuid-A676F8A876F87A7F/
BUR2T=/srv/dev-disk-by-uuid-0A72FB3772FB25DB/
SDCARD=/srv/dev-disk-by-uuid-DAE47F04E47EE265/
BACKUP_MINIPC_D=/srv/dev-disk-by-uuid-5E10649B10647C41/BACKUP-MINIPC-D/


## MINIPCの$D/gitの下にbare repository 新規作成

### サーバー側
cd $D/git
git init --bare --shared NAME.git
git config --global --add safe.directory /srv/dev-disk-by-uuid-B45EDCD75EDC9388/git/NAME.git

### クライアント側
cd git
git clone saka1029@minipc.local:/srv/dev-disk-by-uuid-B45EDCD75EDC9388/git/NAME.git 
