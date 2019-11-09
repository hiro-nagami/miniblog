---
title: "Android Emulator Bad Network"
date: 2019-11-09T15:30:11+09:00
draft: false
tags: [ "android", "emulator", "network" ]
---

## Androidエミュレータでネットワークがつながらない

どうもDNSにアクセスができなくなってしまうらしい。

Android SDKディレクトリにあるemulatorコマンドを使うことでリセットできる。

```${SDK PATH}/emulator/emulator -list-avds```

```${SDK PATH}/emulator/emulator -avd Pixel2_API_27 -dns-server 8.8.8.8```