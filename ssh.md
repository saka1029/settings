# ssh

## パスワードなしでログイン

### クライアント側

```
ssh-keygen -t rsa
```

パスフレーズは空でよい(Enterを2回押す)
公開鍵~/.ssh/id_rsa.pubが作成される。

### サーバー側
作成されたクライアントの id_rsa.pub をサーバにコピーし、
```
~/.ssh/authorized_keys
```
に追加する。
unixの場合はクライアントで以下を実行すればよい。
```
ssh-copy-id -i ~/.ssh/id_rsa.pub  saka1029@minipc.local
```


