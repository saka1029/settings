# vscode

## vim設定

インサートモードでESC押下時にIMをオフにする。
インサートモード開始時に前回のIM状態を引き継がない。

### Windows
```
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.defaultIM": "0",
"vim.autoSwitchInputMethod.obtainIMCmd": "C:\\Users\\saka1\\git\\util\\bin\\zenhan.exe 0",
"vim.autoSwitchInputMethod.switchIMCmd": "C:\\Users\\saka1\\git\\util\\bin\\zenhan.exe {im}",
```
以下のようにするとIM状態を引き継ぐ。
```
"vim.autoSwitchInputMethod.obtainIMCmd": "C:\\Users\\saka1\\git\\util\\bin\\zenhan.exe",
```

### Linux
```
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.defaultIM": "-c",
"vim.autoSwitchInputMethod.obtainIMCmd": "/usr/bin/fcitx-remote",
"vim.autoSwitchInputMethod.switchIMCmd": "/usr/bin/fcitx-remote {im}",
```

### Ubuntu(ibus)
```
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.defaultIM": "xkb:us::eng",
"vim.autoSwitchInputMethod.obtainIMCmd": "/usr/bin/ibus engine",
"vim.autoSwitchInputMethod.switchIMCmd": "/usr/bin/ibus engine {im}",
```

## vimで日本語入力(v1.101以降)

Ctrl+, で settings.json を開き、以下を設定する。
```
"editor.editContext": false
```

## Androidでダウンロード

```
wget -O vscode.deb "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-arm64"
```

## markdown イメージ貼り付け

```
"markdown.copyFiles.destination": {
    "**/*": "${documentBaseName}/${documentBaseName}.${fileExtName}"
},
```

## 起動時にkeyringの認証ダイアログが表示されるとき

~/.local/share/keyrings/login.keyring ファイルを削除して、改めて VS Code を起動し直す。

そうすると以下のような新規にパスワードを設定する画面が表示されます。このとき、
パスワードを「空」にする。

## 行数カウント

findコマンドで-execオプションの最後を`+`にするとうまくいく。

```
find git/declisp/src/ -name '*.java' -exec wc {} +
```

```
    9    23   266 git/declisp/src/main/java/saka1029/declisp/DecLispException.java
   11    22   199 git/declisp/src/main/java/saka1029/declisp/Procedure.java
    7    14   110 git/declisp/src/main/java/saka1029/declisp/Applicable.java
   33    91   711 git/declisp/src/main/java/saka1029/declisp/CodePointBuffer.java
  100   501  4270 git/declisp/src/main/java/saka1029/declisp/Operators.java
  168   493  4588 git/declisp/src/main/java/saka1029/declisp/Reader.java
  279  1198 13062 git/declisp/src/main/java/saka1029/declisp/DecLisp.java
   84   395  3041 git/declisp/src/main/java/saka1029/declisp/Common.java
   33   130   953 git/declisp/src/main/java/saka1029/declisp/Env.java
   17    37   280 git/declisp/src/main/java/saka1029/declisp/Nil.java
   45   145  1501 git/declisp/src/main/java/saka1029/declisp/Cons.java
   33    85   664 git/declisp/src/main/java/saka1029/declisp/Bool.java
   66   174  1860 git/declisp/src/main/java/saka1029/declisp/Expr.java
   37    77   744 git/declisp/src/main/java/saka1029/declisp/Dec.java
   14    28   245 git/declisp/src/main/java/saka1029/declisp/Symbol.java
   30    52   722 git/declisp/src/test/java/saka1029/declisp/TestCommon.java
   44    98  1238 git/declisp/src/test/java/saka1029/declisp/TestCons.java
   21    38   478 git/declisp/src/test/java/saka1029/declisp/TestBool.java
   19    36   373 git/declisp/src/test/java/saka1029/declisp/TestNil.java
   23    49   525 git/declisp/src/test/java/saka1029/declisp/TestApplicable.java
   52   124  1335 git/declisp/src/test/java/saka1029/declisp/TestCodePointBuffer.java
   99   238  3076 git/declisp/src/test/java/saka1029/declisp/TestReader.java
   76   173  2434 git/declisp/src/test/java/saka1029/declisp/TestExpr.java
   29    56   659 git/declisp/src/test/java/saka1029/declisp/TestEnv.java
   14    23   249 git/declisp/src/test/java/saka1029/declisp/TestSymbol.java
   84   316  2939 git/declisp/src/test/java/saka1029/declisp/TestOperators.java
   16    24   317 git/declisp/src/test/java/saka1029/declisp/TestDecLispException.java
  474  1995 20763 git/declisp/src/test/java/saka1029/declisp/TestDeclisp.java
 1917  6635 67602 合計
```

## コンパイルがうまく行かないとき

VS Codeの場合は、コマンドパレット（Ctrl+Shift+P）を開き、「Java: Clean Java Language Server Workspace」を実行します。
