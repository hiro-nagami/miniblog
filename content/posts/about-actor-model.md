---
title: "About Actor Model"
date: 2019-03-09T08:55:36+09:00
draft: false
---

## Actorモデルとは？
インフラ環境などでよく使われる並列処理を実現する方法の１つです。

1973年にカール・ヒューイット氏によって発表されました。オブジェクト指向が「全てのものはオブジェクト指向である」というのに対し、アクターモデルは「全てのものはアクターである」という哲学に基づいて設計されています。

## 「すべてのものはアクターである」とは
アクターとは、メッセージパッシングによって処理の送受信を行うオブジェクトのことを言います。アクターモデルは、いくつものアクターで構成されます。

アクター同士のやり取りは、基本的にメッセージパッシングで行われるためやり取りの到着順は保証されない特徴があります。その分、スケールや大量の処理を素早く行うことができるというメリットもあります。

# アクターモデルの並列性
オブジェクト指向は１つの処理を逐次的に行う事に対して、アクターモデルはメッセージパッシングで処理の送受信を行うため、他の処理を待つことがありません。そのため、アクターモデルは並列的な分散処理と言われています。