---
title: "JavaのExecutorの概要と動き"
date: 2019-03-20T09:12:05+09:00
draft: false
description: Java上でマルチスレッド実現しやすくするためのインターフェースExecutorServiceの概要を簡単に解説して行きます。
keywords: "android, java, thread, ExecutorService"
tags: [ "android", "java" ]
---

## ExecutorSeriviceとは
Java上でマルチスレッド実現しやすくするためのインターフェースです。Runnableインターフェースを実装したインスタンスをスレッドのQueueに追加(`submit()`)することができます。ExecutorServiceは`java.util.concurrent`に含まれます。

<!--more-->

## どういう役割なのか
複数のスレッドを管理しやすくしてくれる。起動するスレッド数を制限したり、スレットセーフにタスクを処理することをサポートしてくれるインターフェースです。

## 使い方
1. タスクを実行するためのスレッドの作成する。作ることができるスレッドの種類は以下のようなものがある
    - SingleThreadExecutor
    - FixedFixedThreadPool
    - CachedThreadPool
    - ScheduledThreadPool

## タスクの状態
タスクには4つの状態があります。

- [created] ThreadのQueueに追加(submit)される前の状態
- [submitted] ThreadのQueueに追加(submit)された状態
- [started] タスクが実装されている状態
- [completed] タスクが完了した状態

## まとめ
ExecutorSeriviceは、タスクをキューイングできる`ThreadPool`を生成することができ、マルチタスクを簡単かつセーフティに扱うことをできるようにするものでした。マルチタスクを実現したい時に使うことをお勧めです。