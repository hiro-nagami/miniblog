---
title: "Cannot Open Xcworkspace With Catalina"
date: 2019-11-09T16:44:09+09:00
draft: false
tags: [ "xcode", "workspace", "catalina" ]
---

## CatalinaでXcode Projectが開かない

リリースされてからある程度時間もたったので、CatalinaにアップデートしたところXcode Projectが開かなくなってしまった。Xcodeは起動できるが、特定のプロジェクトを開こうとするとすぐにXcodeが終了してしまう。

## まずは他のプロジェクトを動かす

前に作ったサンプルプロジェクトがあったので、起動してみたらプロジェクトが開けた。先ほどのプロジェクトは、xcworkspaceで今回のはxcodeprojという違いがあったので、元のプロジェクトのxcodeprojを開いてみたら開けた。

## CocoaPodsで再ビルド

xcworkspaceはCocoaPodsで生成されるため、とりあえずpod installを試してみた。しかし、再度開いても結果は同じだった。

そのため、今度はxcworkspaceを削除してからpod installを試してみたところ無事開けた。