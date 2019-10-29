---
title: "App Itunes Release"
date: 2019-10-29T14:27:12+09:00
draft: false
tags: [ "ios", "release", "certificate" ]
---

## アプリリリースするための手順
アプリをリリースするためには、以下の作業が必要になる。
- App Storeにアプリを登録する
- 申請用のバイナリをアップロードする
- アプリレビュー申請

### App Storeにアプリを登録する
リリースするためにApp Store Connectでアプリを作成する。そのために以下の項目を埋めていく。
- App Information
    - App Storeに表示されるアプリ名
    - アプリのカテゴリ設定
    - プライバシーポリシーの設定
        - テンプレートなどもあるが、可能なら専門の人に作ってもらうのがベスト
- Pricing And Availability
    - 課金の設定
    - リリース前の事前登録の設定
- アプリの詳細設定
    - スクリーンショットの追加
        - [App preview specifications](https://help.apple.com/app-store-connect/#/dev4e413fcb8)
        - [[Xcode] iOS シミュレータで、スクリーンショットを保存する時は、⌘ + S で OK！](https://rakuishi.com/archives/4634/)
        - [Images can't contain alpha channels or transparencies](https://stackoverflow.com/questions/25681869/images-cant-contain-alpha-channels-or-transparencies)
    - App Storeに表示するアプリ説明
    - 検索キーワード
    - プライバシーポリシー・サポートサイトの登録
    - バイナリーのアップロード
    - App Store用アイコン、アプリバージョン、Ratingの設定
    - コピーライト
        - [著作権表示、コレが正解！「©」や「All Rights Rserved」正しい表記と意味全解説](https://nandemo-nobiru.com/om-6265)
    - 開発者情報の設定
    - レビューアカウント情報の設定
    - 自動リリースか手動リリースかの設定

### 申請用のバイナリをアップロードする
アプリを申請するために必要なバイナリのアップロード手順は次の通り。
- AppStore用 CertificateとProvisioningの作成
- Bundle IDの設定
- リリースバージョンの設定
- `Generic iOS Device`を選択してアーカイブ
- アーカイブしたバイナリを選択して、`Validate App`でバイナリ検証
- 検証したバイナリを選択して、`Distribute App`でApp Storeにアップロード
- App Store Connectのアプリページでアップロードしたバイナリを申請用に選択

### アプリレビュー申請
上記の2つが終わったら、App Storeへ出すためのアプリレビューに申請する。この申請が通れば、アプリをリリースすることができる。
- アプリ詳細ページで`Submit and Review`ボタンを押す
- 事前設定項目を埋める
    - [輸出コンプライアンス](https://www.webbanana.org/goroku/2019/06/12/374.php)
    - [[2018年版] よく分かる！iOSアプリのリリース手順のまとめ](https://dev.classmethod.jp/smartphone/iphone/ios-app-how-to-release-2018/)
    - [【2018年度版】アプリ開発における「年度末自己分類報告」の作成と提出方法の調査結果](https://akira-kobe.hatenablog.com/entry/2018/05/03/205926)
