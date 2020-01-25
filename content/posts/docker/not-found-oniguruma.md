---
title: "Not Found Oniguruma"
date: 2020-01-21T18:47:35+09:00
draft: false
tags: [ "docker", "oniguruma", "alpine" ]
---

## Dockerでimageビルドしていたらエラー
Dockerビルドしていたら、onigurumaが見つからなくてエラーが出ていた。原因としては、PHP 7.4.0から onigurumaが内包されないようになったかららしい。

```
Package 'oniguruma', required by 'virtual:world', not found
```

- http://tekitoh-memdhoi.info/views/807
- https://qiita.com/ucan-lab/items/14201de8fcf3c5251c12


## 対策
apkでonigurumaをインストールする
```
$ apk add --update --no-cache oniguruma-dev
```
