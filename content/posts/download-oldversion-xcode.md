---
title: "Download Oldversion Xcode"
date: 2019-11-09T16:38:00+09:00
draft: false
tags: [ "swift", "xcode" ]
---

## Xcodeアップデートしたらコンパイラがアップデートしていて動かない

Xcodeをアップデートしたら、Swiftのバージョン(5.1.2)に上がっていたためCarthageでビルドしたフレームワークが動かなくなってしまった。

![image](https://user-images.githubusercontent.com/25496478/68525084-3a8f0400-0311-11ea-94f7-d36400e8a2cf.png)

対処法は２つ

1. Carthageでビルドし直す。対応したバージョンがリリースされていればそれを使うし、なければローカルで再ビルドされる
2. 元のSwiftバージョン(5.1.0)で使いたいので旧バージョンを落として動かす

とりあえず、元のSwiftバージョン(5.1.0)で使いたいので旧バージョンを落としてきて対処。

古いバージョンを使い続けるのは、技術的な負債になるのでなるべく早くアップデートに対応したい。

https://developer.apple.com/download/more/