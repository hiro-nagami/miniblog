---
title: "iOS App Debugging Crashreport"
date: 2019-10-31T12:52:50+09:00
draft: false
tags: [ "ios", "crash", "symbolicate" ]
---

AppleなどからもらったクラッシュファイルをSymbolicateする方法は以下の通り。

1. クラッシュファイルを手に入れる
2. dSYMを手に入れる
3. symbolicateする

### txtファイルの場合

`symbol.txt` を　`symbol.crash`　のように拡張子を変更する。

### dSYMの取得

App Store ConnectかArchivesから取得する。

1. App Store Connect

```
Activity -> Build Number -> Included Symbols
```
から取得可能。

2. Archives

Archiveのパッケージ内に存在するのでそれを利用する。
```
Window -> Organizer -> Select Archive -> Show in finder -> Open package
```

## symbolicateする

以下にある`symbolicatecrash`というコマンドを使う。

```sh
/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash
```

適当なフォルダを作り、そこにクラッシュファイルとdSYMを一緒にいれる。

```sh
$ cd tmp
$ ls
symbol.crash
app.dSYM
framework.dSYM
```

`DEVELOPER_DIR`をexportする

```sh
$ export DEVELOPER_DIR="/Applications/Xcode.app/Contents/Developer"
```

クラッシュファイルに対して`symbolicatecrash`コマンドを実行する。

```sh
./symbolicatecrash symbol.crash > symbolicated.crash
```

これでクラッシュファイルが読めるようになる。