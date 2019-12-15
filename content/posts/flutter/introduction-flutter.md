---
title: "Introduction Flutter"
date: 2019-12-11T15:26:04+09:00
draft: false
tags: [ "flutter", "introduction" ]
---

## Flutter開発環境の構築

### Flutter をインストール

今回はMacOSバージョン。`$HOME/flutter`に配置。

- https://flutter.dev/docs/get-started/install/macos

### 初期設定

1. `PATH=$PATH:$HOME/flutter/bin`
1. `flutter precache`
1. `“idevice_id”は、開発元を検証できないため開けません。`が出る事があるので以下のコマンドで対処
    - `udo xattr -d com.apple.quarantine $HOME/flutter/bin/cache/artifacts/libimobiledevice/idevice_id`
    - `udo xattr -d com.apple.quarantine $HOME/flutter/bin/cache/artifacts/libimobiledevice/ideviceinfo`
    - 参考：https://yaba-blog.com/flutter-idevice_id/
1. `flutter doctor`でチェックし、必要なセットアップをする

### Android
1. Android Studioを入れる
1. Android Studio Pluginsにflutter/dart pluginを入れる

### iOS


### VSCode
1. VSCodeを入れる
1. VSCodeにflutter pluginを入れる