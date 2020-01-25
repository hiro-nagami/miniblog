---
title: "Add GD and ImageMagick for PHP-FPM on Docker"
date: 2020-01-26T00:26:34+09:00
draft: false
tags: [ "docker", "gd", "imagemagick", "php-fpm", "alpine" ]
---

## Dockerで alpine: php-fpm にGDとImageMagickを導入
WordPressを設定している時に画像のS3アップロードでGD or ImageMagickが必要になったので導入方法を調べてみた。

## GD
dockerでalphineイメージを使ってGDを導入する場合は、docker-ext-configure と docker-php-ext-install を使うとphpでも有効になる。またGDを使うには、いくつかimage系のライブラリが必要だった。 `--virtual` は仮の名前をつけてインストールすることができ、指定した名前を使うことで一括削除することもできる。

```
RUN apk add --update --no-cache --virtual .ext-deps \
        libjpeg-turbo-dev \
        libwebp-dev \
        libpng-dev \
        freetype-dev \
        libmcrypt-dev
apk del .ext-deps

docker-php-ext-configure gd \
    --with-jpeg=/usr/include \
    --with-webp=/usr/include \
    --with-freetype=/usr/include && \
docker-php-ext-install gd
```

## ImageMagick
dockerでalphineイメージを使ってImageMagickを導入する場合は、ImageMagickをPHPに適用する必要がある。適用するには docker-php-ext-enable と imagick を利用する。imagemagickをインストールするのに gcc/g++/autoconf/make が必要になるので、それもインストールする。`apk add build-base`コマンドでも解消可能。

```
RUN apk add --update --no-cache --virtual .imagemagick-deps \
    gcc \
    g++ \
    autoconf \
    make && \
    pecl install imagick && \
    docker-php-ext-enable imagick
```

- [Dockerなphpの環境でimagickをinstallする](https://polidog.jp/2018/05/08/php-docker-imagick/)
- [PHP & Docker & AlpineLinux な環境でImageMagickを使用する方法（＋Laravelで画像処理をする方法）](https://system-j.net/laravel/114/)
- [php-alpineコンテナにxdebugをインストールする時にハマったメモ](https://qiita.com/ucan-lab/items/fbf021bf69896538e515)
- [nginx と PHP-FPM の仕組みをちゃんと理解しながら PHP の実行環境を構築する](https://qiita.com/kotarella1110/items/634f6fafeb33ae0f51dc)