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
