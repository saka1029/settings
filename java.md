# jdk 21 インストール
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.deb

# バージョン切り替え
$ sudo update-alternatives --config java
alternative java (/usr/bin/java を提供) には 3 個の選択肢があります。

  選択肢    パス                                       優先度  状態
------------------------------------------------------------
  0            /usr/lib/jvm/java-21-openjdk-amd64/bin/java   2111      自動モード
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java   1111      手動モード
* 2            /usr/lib/jvm/java-17-openjdk-amd64/bin/java   1711      手動モード
  3            /usr/lib/jvm/java-21-openjdk-amd64/bin/java   2111      手動モード

現在の選択 [*] を保持するには <Enter>、さもなければ選択肢の番号のキーを押してください: 3
update-alternatives: /usr/bin/java (java) を提供するためにマニュアルモードで /usr/lib/jvm/java-21-openjdk-amd64/bin/java を使います
