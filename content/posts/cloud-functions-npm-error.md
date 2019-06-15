---
title: "nodeバージョンの違いでFirebase Cloud Functionsのdeployが失敗した話"
date: 2019-06-15T19:33:52+09:00
draft: false
description: 'cloud functionsをデプロイしようとすると HTTP Error: 400, The request has errors という内容の不具合が発生しときに対処した話。'
keywords: "cloud function, npm, nodejs, エラー, error"
---

## nodeのバージョンが違ったことでdeployが失敗

今作っているプロダクトで、Firebase Cloud Functionsを使うことになったので試してみようと以下のプログラムをデプロイしてみた。

```js
import * as functions from 'firebase-functions';

export const hello = functions.
    https.onRequest((request, response) => {
    response.send("Hello, firebase cloud functions");
});
```

すると、以下の内容の不具合が発生した。

```sh
❯ npm run deploy

> functions@ deploy ~/functions
> firebase deploy --only functions


=== Deploying to 'spofun-dev'...

i  deploying functions
Running command: npm --prefix $RESOURCE_DIR run lint

> functions@ lint ~/functions
> tslint -p tslint.json

no-use-before-declare is deprecated. Since TypeScript 2.9. Please use the built-in compiler checks instead.
no-unused-variable is deprecated. Since TypeScript 2.9. Please use the built-in compiler checks instead.
Running command: npm --prefix $RESOURCE_DIR run build

> functions@ build ~/functions
> tsc

✔  functions: Finished running predeploy script.
i  functions: ensuring necessary APIs are enabled...
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (71.23 KB) for uploading
✔  functions: functions folder uploaded successfully
i  functions: creating function hello...
⚠  functions: failed to create function hello
HTTP Error: 400, The request has errors


Functions deploy had errors. To continue deploying other features (such as database), run:
    firebase deploy --except functions

Error: Functions did not deploy properly.
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! functions@ deploy: `firebase deploy --only functions`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the functions@ deploy script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/h-nagmai/.npm/_logs/2019-06-12T06_19_44_689Z-debug.log
```

エラーの内容がざっくり過ぎて、何が問題なのかわからず結構ハマった。原因がわかるきっかけになったのは以下のコマンドを実行したとき。Nodeのバージョンに比べて、firebase-toolsのバージョンが古かったため。

```sh
❯ npm run shell

> functions@ shell ~/functions
> npm run build && firebase experimental:functions:shell


> functions@ build ~/functions
> tsc

⚠  functions: Cannot start emulator. Error: The module '/usr/local/lib/node_modules/firebase-tools/node_modules/grpc/src/node/extension_binary/grpc_node.node'
was compiled against a different Node.js version using
NODE_MODULE_VERSION 57. This version of Node.js requires
NODE_MODULE_VERSION 67. Please try re-compiling or re-installing
the module (for instance, using `npm rebuild` or `npm install`).
```

## 対処法

firebase-toolsのバージョンを、入っているnodejsのバージョンで動くものにアップデートをして対処できた。

```sh
npm install firebase-tools@latest
```

