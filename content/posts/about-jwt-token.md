---
title: "JWTについて簡単に調べたのでメモ書き"
date: 2019-06-16T00:22:40+09:00
draft: false
description: JWTについて調べた際の簡単なまとめ
keywords: "jwt, security"
---


JSON Web Tokenの略でURL-SafeなTokenを発行し、電子署名をJWTに組み込むことで改ざんを検出可能にする。

JWTは以下の内容で構成される

- Base64-Header: ヘッダー情報を含むJSONをBase64でエンコードしたもの
- Base64-Body: ユーザーID, 有効期限をJSONをBase64でエンコードしたもの}.{電子署名}
- Signature: 電子署名

```
JWT: {Base64-Header}.{Base64-Body}.{Signature}
```

サーバー側は自身の持つ暗号鍵を使って署名つきのTokenを発行することで、有効期限とユーザーIDの正当性を検証し改ざんがあれば検知できるようになっている。URL-Safeなので文字コードに依存せずウェブ上の通信の本人証明に使うことができる。

## JWK (JSON Web Key)
JWTに含まれるクレームの値を使って取得することができるJWKを使って、JWTのSignatureを検証することができる。

## 参考

- [JSON Web Token の効用](https://qiita.com/kaiinui/items/21ec7cc8a1130a1a103a)
- [JWT(JSON Web Token)を使った認証を試みる](https://blog.kazu69.net/2016/07/30/authenticate_with_json_web_token/)
- [JWT について調べた内容をまとめました。](https://qiita.com/gctoyo/items/8d0ffb265845ab8cc87c)
- [JWT(JSON Web Token)の仕組みと使い方まとめ](https://webbibouroku.com/Blog/Article/jwt)
- [OAuth Revocation と JWK を翻訳しました](https://oauth.jp/blog/2016/07/01/jwk-and-oauth-revocation-translated/)
- [ID トークンの確認](https://firebase.google.com/docs/auth/admin/verify-id-tokens?hl=ja)
- [JSON Web Key (JWK)](https://openid-foundation-japan.github.io/rfc7517.ja.html)