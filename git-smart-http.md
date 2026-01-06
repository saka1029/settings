# Smart HTTP

git-http-backend(1) は CGI プログラムで、HTTP(S) で clone や pull, push を効率的に行うことができます。

## Apache

Apache HTTP Server をインストールし、mod_cgi, mod_alias, mod_env を有効にして、そしてもちろん git があれば、この設定はかなり簡単です。
```
sudo apt-get install apache2 apache2-utils
a2enmod cgi alias env rewrite
```

基本的なセットアップを実行したら、Apache の設定ファイルに以下を追加してください。

/etc/httpd/conf/httpd.conf

(/etc/apache2/apache2.conf)
```
<Directory "/usr/lib/git-core">
    Require all granted
</Directory>
 
SetEnv GIT_PROJECT_ROOT /srv/git
SetEnv GIT_HTTP_EXPORT_ALL
ScriptAlias /git/ /usr/lib/git-core/git-http-backend/
```

ここでは、Git リポジトリが /srv/git にあり、それにアクセスするために次のような方法をとるものとします。http(s)://your_address.tld/git/your_repo.git.
ノート
Apache がリポジトリに対して読み書きができることを確認してください。

より詳細なドキュメントについては、以下のリンクを参照してください。

    https://git-scm.com/book/en/v2/Git-on-the-Server-Smart-HTTP
    https://git-scm.com/docs/git-http-backend

