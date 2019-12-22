---
title: "Colaboratory Introduction"
date: 2019-12-22T19:46:29+09:00
draft: false
tags: [ "machine-learning", "colaboratory" ]
---

## Google Colabを使った機械学習
Google Colabとは、Google が提供している機械学習をオンライン上で実行することができる開発環境。

- https://colab.research.google.com/


### Uploaderの表示や桁数の多い計算なども簡単にできる

<img width="455" alt="スクリーンショット 2019-12-22 20 05 35" src="https://user-images.githubusercontent.com/25496478/71320978-8dee9780-24f6-11ea-960a-29d33fdc4861.png">

## scikit-lean
Python向けの機械学習フレームワークサイト。機械学習初心者の人は使用できるアルゴリズムの種類なども知らないので、scikit-learn algorithm cheatsheetsを参考にするのがおすすめ。scikit-learn algorithm cheatsheetsによると基本的に50以上のデータがるのが望ましい。

## And/Orを機械学習させる
今回はscikit-learnからsklearn.svmのLinearSVCとsklearn.metricsのaccuracy_scoreを利用する。

AND 演算
<img width="441" alt="スクリーンショット 2019-12-22 20 21 24" src="https://user-images.githubusercontent.com/25496478/71321142-f76fa580-24f8-11ea-8cc6-8c078785dee7.png">

XOR 演算
<img width="414" alt="スクリーンショット 2019-12-2 2 20 22 39" src="https://user-images.githubusercontent.com/25496478/71321144-fcccf000-24f8-11ea-8521-caf90aee0b82.png">

ANDは正しく学習できたが、XORは学習できていない。
別なアプローチをする。パラメータを増やすか、アルゴリズムを変更するか。
アルゴリズムを変更するアプローチをとるために、cheatsheetsをみる。
KNeighborsClassifierを利用する。すると正しく結果が出る。

<img width="471" alt="スクリーンショット 2019-12-22 20 29 04" src="https://user-images.githubusercontent.com/25496478/71321217-cd6ab300-24f9-11ea-831d-a05a4516c7f8.png">




