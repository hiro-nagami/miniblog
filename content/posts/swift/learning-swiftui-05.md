---
title: "Learning Swiftui 05"
date: 2019-12-07T02:11:22+09:00
draft: false
tags: [ "ios", "swiftui", "swift" ]
---

## ゴール
SwiftUIで使われいるBindingについて理解する

## 現状
先日@Stateについては少し理解したが、SwiftUIを使っていると時々Bindingプロパティを要求されることがあるが理解できていない。

## Stateプロパティについて
Viewはstructのため値を変更できないが、@StateをつけることによってSwiftUI側でメモリ管理を移譲することができる。それを利用して値の更新ができるようになる。この変更時に、内部にあるビューを再構築してくれる。この値は外部から変更することができない。

## SwiftUIのObservableについて
`ObservableObject`  プロトコルに適合していると `@ObservedObject` を宣言したプロパティへ変更通知することができる。監視したいプロパティに `@Published` を宣言すると自動で変更を監視してくれる。

## Property Wrappersについて
`@` を仕様した宣言をすることで独自のアクセスを提供する。`@State` `@ObservedObject` `@Published` などがあげられる。

## 参考
https://qiita.com/shiz/items/6eaf87fa79499623306a
https://developer.apple.com/videos/play/wwdc2019/226/