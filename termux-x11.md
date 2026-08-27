Android端末にLinuxの環境を構築する(②Ubuntuのインストール)
2024年6月30日 2:27 PM011.91KモバイルコンピューターAndroidLinuxTermuxUbuntu
目次
目次を隠す

    proot-distroのインストール
    Ubuntuのインストール
    GUI環境の構築
    termux:x11の設定
    ソフトウェアキーボードの導入(Unexpected-Keyboard)

総合目次はこちら
proot-distroのインストール

以下のコマンドを実行し、proot-distroをインストールする

$ pkg install proot-distro

Ubuntuのインストール

以下のコマンドを入力することで、インストール可能なディストリビューションの一覧が見れる

$ proot-distro list

色々あって迷うが、無難にUbuntu (22.04)のパッケージをインストールしていく。
インストールとログイン

    以下のコマンドを入力

    $ proot-distro install ubuntu

    ログインする

    $ proot-distro login ubuntu

    パッケージを最新化する

    # apt-get update
    # apt-get upgrade -y

    デフォルトのシェルをbashに変更

    # chsh -s /bin/bash

    sudo, vim のインストール

    $ apt install sudo vim

    途中国と地域を聞かれるので、適宜入力

ユーザの追加 変更

    以下のコマンドを入力し、ユーザの追加・パスワードの登録・sudo権限の追加を行う

    $ useradd -m XXXXXX
    $ passwd XXXXXX
    $ usermod -aG sudo XXXXXX

    赤字の部分は任意のユーザ名となる。
    sudoersファイルの編集

    $ visudo

    次の行を追加する
    /etc/sudoers.d
    1
    	
    XXXXXX  ALL=(ALL:ALL) ALL
    一旦ログアウトする。

    $ exit

    Termuxのコンソールに戻るので、先ほど登録したユーザで再度ログインする

    $ proot-distro login ubuntu --user XXXXXX

日本語ロケールの設定

    日本語のランゲージパックをインストールする

    $ apt install language-pack-ja -y

    .bashrcファイルの末尾に以下2行を追加する
    .bashrc
    1
    2
    	
    export LANG=ja_JP.UTF-8
    export LANGUAGE="ja_JP:ja"
    再度ログアウトして、ログインし直す

その他諸々

    sudo apt-get install software-properties-common

    sudo apt-get install font-manager

GUI環境の構築

Ubuntuのデスクトップ環境として、今回はXfce4を使用する

    一旦Ubuntuからログアウトして、Termux上で以下のコマンドを実行する。

    $ pkg install x11-repo
    $ pkg install termux-x11-nightly

    以下のサイトよりTermux-x11の最新版apkをダウンロードしてインストールを行う。
    https://github.com/termux/termux-x11/releases
    F-Droidをインストールした時と同様に警告等は全て許可して進めていく。
    Xfce4をインストールする

    $ sudo apt install -y xfce4 xfce4-goodies dbus-x11

    途中国とキーボードを聞かれるので、適宜入力
    ソフトウェアキーボードで運用する場合は英語を選んでおいた方が良い。
    起動用のスクリプトの作成する。
    直接コマンドを打ち込んでも良いが今後のためにもスクリプト化しておく。
    こちらのスクリプトは今後色々とカスタマイズしていく事となる。
    ./x11
    1
    	
    termux-x11 :0 -xstartup "dbus-launch --exit-with-session xfce4-session"
    スクリプトをホームディレクトリにx11という名前で保存し、実行権限を付加後に実行し、x11サーバを起動した状態にする。

    ./x11

    Termuxの画面は一旦そのままにしてAndroidのホーム画面に戻り、Termux:x11 のアイコンをタップして起動する。
    Xfce4のデスクトップ画面が表示されたら成功。
    Xfce4のデスクトップ画面

termux:x11の設定
権限の設定

adbを使用してtermux:x11にWRITE_SECURE_SETTINGSの権限を追加する

adb shell pm grant com.termux.x11 android.permission.WRITE_SECURE_SETTINGS

設定内容

Termux:x11の設定は画面上からスワイプすると出てくる通知一覧の中から設定を行う。
主に以下の設定を変更しています。
記載されている内容はgoogle lens+翻訳を参考にしているで若干間違っている可能性あります。
Termux:x11設定内容(抜粋) 名称	設定値	説明
【Output】セクション
Display resolution mode	画面の解像度	native	
Reseed screen while soft keyboard is open	ソフトウェアキーボードが開くときに画面サイズを再調整する。	ON	OFFだとボタンなどがソフトウェアキーボーの下に隠れたり扱いづらいのでON推奨。
Fullscreen	フルスクリーン	ON/OFF	これがONになっているとソフトウェアキーボードの種類によっては上述のReseed screen while soft keyboard is openが効かない。
なのでソフトウェアキーボードで運用する場合、OFFにしているが、それ以外はON。
【Pointer】セクション
Touchscreen input mode	タッチスクリーンモード	Tracpad	
Captured pointer speed factor, %	ポインタの移動速度	50%	ポインタの移動速度を設定する項目だと思うがなぜか変化なし。
【Keyboard】セクション
Prefer scancodes when possible	可能であればスキャンコードを使用する	ON	
Enable Accessibility service for intercepting system shortcuts automatically.	ユーザ補助機能を利用してシステムのショートカットに割込みます	ON	ハードウェアキーボードを使用する場合は必ずONに。
【Other】セクション
Clipboard sharing	クリップボード共有	ON	
ソフトウェアキーボードの導入(Unexpected-Keyboard)

Termuxをソフトウェアキーボードで使用する場合、Gboardなど通常のキーボードアプリはALTやCTRLなどの制御キーを持たないためショートカットなどの操作が出来ない。
Termux-x11のAdditional Keyboardを有効にするという方法もあるが、今回は別途特殊キーに対応しているソフトウェアキーボードをインストールします。

他のサイトだとCordBoardだったりHacker's Keyboardを推している人が多いですが、自分はUnexpected-Keyboardがキーが大きくて使いやすいので使用しているのでそちらの導入と設定を行う。

    Google PlayからUnexpected-Keyboardをインストールする。
    以下の通り設定。
    Unexpected-Keyboard設定内容(抜粋) 名称	設定値	説明
    【Layout】セクション
    layout1	レイアウト	QWERTY(US)	
    Show number row	数字行の表示	OFF	
    Show NumPad	NumPadの表示	Only in landscape mode	横画面の時だけ表示するように設定
    【Typing】
