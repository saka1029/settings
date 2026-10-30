# github

## ソースコードの行数を数える

```
find src -name '*.java' | xargs wc -l
```

## GitHubにある全てのレポジトリ(2026-10-30現在)

```
CSP-J
CSPINT
Common
Deep.Learning
GIS
GPSLog
Geographics
IllegalShapedLand
JavaCSP
JuLisp
LambdaCalculus
MiniSat-J
Notype
Obscure
Puzzle
SchemeMinimum
SchemeWithContinuation
Terrain
compiler
csp
declisp
device
executable
form
golisp
iterable
lambda
manual
nested
pcstack
rosetta
rust
s
settings
shinryo
spec
stack
stackoverflow
tensuhyo
util
```

## コマンドラインで一覧を取得

```
gh repo list saka1029 --limit 1000
```

ghコマンドは以下で取得する。
```
sudo apt install gh
```
インストールしたら以下を実行する。
```
gh auth login
```
`Paste authenticate token`を選択し、
GitHubのauthenticate tokenを貼り付ける。
