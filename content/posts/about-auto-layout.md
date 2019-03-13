---
title: "SketchのAutoLayout機能"
date: 2019-03-13T16:49:14+09:00
draft: false
---

SketchでAutoLayoutを使えるようにするプラグイン[Anima ToolKit](https://www.animaapp.com/)を使ってみました。

## AutoLayoutとは何か？
制約を設定する事でビューを自動的に計算してリサイズしたり、位置を調整したりする機能。優先度を設定することによって、どのパーツを優先的にリサイズしたり、非表示にしたりするか決める事ができます。

AutoLayoutはStoryboardでビューに設定するのが分かりやすいが、コード上でも設定する事が可能です。

## どういう動きをするのか
AutoLayoutを使うことによって以下のような動きをさせることができます

- オブジェクトのサイズを他のものに合わせてリサイズする
- オブジェクトの位置を他のオブジェクトと合わせる
- オブジェクトを整列させることができる

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="ja" dir="ltr">Sketch Pluginを使ってAutoLayoutのサンプルを作って見た。これは、レスポンシブでもイメージしやすい<a href="https://t.co/cmgHrqt5gG">https://t.co/cmgHrqt5gG</a> <a href="https://t.co/iWHwKktE2Q">pic.twitter.com/iWHwKktE2Q</a></p>&mdash; nagamy (@nagami_hiro) <a href="https://twitter.com/nagami_hiro/status/1086201207574548481?ref_src=twsrc%5Etfw">January 18, 2019</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## まとめ
AutoLayout機能はXcodeでも使うことができる機能です。この機能を理解することで、複数の画面サイズに合わせてウェブやアプリのデザインを効率よく最適な形にしていくことができるようになるでしょう。