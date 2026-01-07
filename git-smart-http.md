# 4.6 Gitサーバー - Smart HTTP
## Smart HTTP

これまでの説明で、SSH を使った認証ありのプロトコルと git:// を使った認証なしのプロトコルについてわかったと思います。続いて、それら両方を実現してしまうプロトコルについて説明しましょう。 Smart HTTP のセットアップは、単に CGI スクリプトをひとつ、Git サーバー上で有効にすればおしまいです。Git に同梱されている git-http-backend というスクリプトを使います。 この CGI は、パスやヘッダー情報（git fetch や git push で特定の HTTP URL 宛に送られてきたデータ）を読み込み、クライアントが HTTP を使ってやりとりできるかどうか判断します（バージョン 1.6.6 以降の Git クライアントであれば対応しています）。 そして、CGI の判断が「このクライアントは Smart HTTP に対応している」だった場合は Smart HTTP が使われ、そうでなかった場合はリードオンリー（“dumb”）にフォールバックします（後方互換という意味では、読み込みについては古いクライアントにも対応しています）。

では、標準的なセットアップ方法について説明しましょう。ここでは、Apache を CGI サーバーとして使います。Apache がインストールされていない場合は、Linux サーバー上で以下のようなコマンドを実行してください。

```
sudo apt-get install apache2 apache2-utils
a2enmod cgi alias env rewrite
```

そうすれば、 mod_cgi、 mod_alias、 mod_env、 mod_rewrite も有効になります。いずれも、Smart HTTP の動作に必要なものです。

また、/opt/git ディレクトリのグループを www-data に変更しなければなりません。CGIスクリプトを実行するApacheのインスタンスはデフォルトではそのグループの1ユーザーとして実行されるからです。設定を変更しておけば、ウェブサーバーは自由にリポジトリを読み書きできるようになります。

```
chgrp -R www-data /opt/git
```

次に、Apache の設定をします。git-http-backend をハンドラにして、ウェブサーバーの /git パスにアクセスがあった場合にそれに処理させるための設定です。

```
SetEnv GIT_PROJECT_ROOT /opt/git
SetEnv GIT_HTTP_EXPORT_ALL
ScriptAlias /git/ /usr/lib/git-core/git-http-backend/
```

環境変数 GIT_HTTP_EXPORT_ALL を設定しない場合、クライアントからのアクセスは読み込み専用になり、読み込めるのは git-daemon-export-ok ファイルが保存されたリポジトリだけになります。Git デーモンと同様の挙動です。

最後に、Apacheの設定を2つ変更します。 git-http-backend へのアクセスを許可する設定と、書き込みを認証するための設定です。Auth ブロックを使う場合、以下のようにして設定できます。

```
RewriteEngine On
RewriteCond %{QUERY_STRING} service=git-receive-pack [OR]
RewriteCond %{REQUEST_URI} /git-receive-pack$
RewriteRule ^/git/ - [E=AUTHREQUIRED]

<Files "git-http-backend">
    AuthType Basic
    AuthName "Git Access"
    AuthUserFile /opt/git/.htpasswd
    Require valid-user
    Order deny,allow
    Deny from env=AUTHREQUIRED
    Satisfy any
</Files>
```

さらに、対象ユーザー全員のパスワードが記述された .htaccess ファイルが必要。ユーザー “schacon” を追加したい場合は、このようなコマンドを実行すｒ。-cオプションはファイルを新規作成する。ユーザを追加する場合は-cオプションを外す。

```
htpasswd -c /opt/git/.htpasswd schacon
```

apache2再起動

```
systemctl restart apache2
```

ユーザー認証を Apache で実施する方法はたくさんあります。 ひとつ選んで設定してください。 ここでは、思いつく限り一番シンプルな方法を説明しました。 また、HTTP 通信が SSL 経由で行われるように設定しましょう。 そうすれば、データはすべて暗号化されます。

ここでは、Apache 設定の詳細についてはあえて立ち入らないようにしました。 Apache 以外の ウェブサーバーを使う場合もあるでしょうし、認証の要求も多様だからです。 覚えておいてほしいのは、Git には git-http-backend という CGI スクリプトが付属していることです。 それが実行されると、HTTP 経由でデータを送受信する際のネゴシエーションを処理してくれます。 このスクリプト自体は認証の仕組みを備えてはいませんが、ウェブサーバーの機能で認証は簡単に管理できます。 CGI に対応している ウェブサーバーであればどれも使っても構いません。一番使い慣れたものを使うのがよいでしょう。

-----

## Smart HTTP

git-http-backend(1) は CGI プログラムで、HTTP(S) で clone や pull, push を効率的に行うことができます。

## Apache

Apache HTTP Server をインストールし、mod_cgi, mod_alias, mod_env を有効にして、そしてもちろん git があれば、この設定はかなり簡単です。
```
sudo apt-get install apache2 apache2-utils
a2enmod cgi alias env rewrite
```
また、/srv/git ディレクトリのグループを www-data に変更しなければなりません。CGIスクリプトを実行するApacheのインスタンスはデフォルトではそのグループの1ユーザーとして実行されるからです。設定を変更しておけば、ウェブサーバーは自由にリポジトリを読み書きできるようになります。

```
sudo chgrp -R www-data /srv/git
```

/srv/gitディレクトリとその下位ディレクトリの所有者と所有者グループ

```
orangepi@orangepizero2w:/srv$ ls -la git
total 12
drwxrwxrwx 3 www-data www-data 4096  1月  6 15:48 .
drwxr-xr-x 3 www-data www-data 4096  1月  6 15:45 ..
drwxrwsr-x 7 www-data www-data 4096  1月  6 15:56 test.git
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

