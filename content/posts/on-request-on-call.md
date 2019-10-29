---
title: "Firebase Cloud FunctionのonCall()とonRequest()のトリガーの違い"
date: 2019-06-17T09:16:29+09:00
draft: false
description: Firebase Cloud FunctionでAPI実装するために使われるトリガーに`onCall()`と`onRequest()`との２つがあったのでそれtについてのメモ。
keywords: "nodejs, firebase, cloud functions"
tags: [ "node", "firebase" ]
---

Firebase Cloud FunctionでAPI実装するために使われるトリガーに`onCall()`と`onRequest()`との２つがあった。

これらは「内部で行なっていること」と「Callbackで受け取れる値」に違いがある。


## onCall()関数
onCall()は、`Content-Type: application/json`ヘッダーを`POST`リクエストのみを受け付けるようになっている。また、`Authentication Bearer`ヘッダーを付与しているとJWTを使ったユーザーの検証を自動で行なってくれるため、呼び出された時点で認証情報が利用できるようになる。

JWTの使用についてはこちら。

[JWTについて簡単に調べたのでメモ書き](https://www.techree.net/posts/about-jwt-token/)

```javascript
export const hello_on_call = functions.https.onCall((data, context) => {
    if (!context.auth) {
        throw new functions.https.HttpsError('Invalid account.');
    }
    return "Hello, world"
});
```

[https.onCall のプロトコルの仕様](https://firebase.google.com/docs/functions/callable-reference?hl=ja)

## onRequest()関数
onRequest()は、POST以外のどのHTTPメソッドでもハンドリングできるようになっている。なので、呼び出されたタイミングでHTTPメソッドを判断して処理を分ける必要がある。
基本的に、内部でユーザーの検証などはないため自前で実装する必要がある。

```js
export const hello = functions.
    https.onRequest((request, response) => {
    cors(req, res, () => {
    const tokenId = req.get('Authorization').split('Bearer ')[1];

    return admin.auth().verifyIdToken(tokenId)
      .then((decoded) => res.status(200).send(decoded))
      .catch((err) => res.status(401).send(err));
  });
});
```
