---
title: "Learning Blockchain 02"
date: 2019-10-26T17:41:43+09:00
draft: false
---

## CryptoZombieをやっためも

### CryptoZombieとは


### 開発環境

開発環境には以下のものを使う。

- Ethereum
    - ブロックチェーン分散プラットフォームを構築するための技術
    - 特徴としてスマートコントラクトを採用している
- [Solidity](https://solidity-jp.readthedocs.io/ja/latest/)
    - スマートコントラクトを扱うオブジェクト指向型の言語
    - https://book.ethereum-jp.net/solidity/
- web3.js
    - Ethereum環境を構築することができるJavaScriptライブラリ
    - https://web3js.readthedocs.io/en/v1.2.2/
- CryptoZombie
    - ブロックチェーン技術を使って「自分のゾンビ生成」「感染」「拡大」をするゲーム
    - このゲームの中で実際にプログラムを書きながら、各機能の実装方法を学びブロックチェーンの全体的な処理を学んでいく
    - Ethereumの実装方法やスマートコントラクトについて学べる

### コントラクトの作成
Solidityを使ってコントラクトを作成する。コントラクトを作成するための記述方法は以下の通り。
```js
contract MyContract {
    // ...
}
```

Solidityのコントラクトではメンバ変数や関数を持つことができる。プライベート関数や引数は、先頭にアンダースコアをつけるのが習わし。また、Structを実装することもできる。


### メンバー変数
```js
contract MyContract {
    uint idLength = 12;
}
```

### Structの実装

```js
contract MyContract {
    uint idLength = 12;

    struct Member {
        string name;
        uint age;
        uint id;
    }
}
```


### 配列

```js
contract MyContract {
    uint idLength = 12;

    struct Member {
        string name;
        uint age;
        uint id;
    }

    Member[] public members;
}
```

### 関数

```js
contract MyContract {
    uint idLength = 12;

    struct Member {
        string name;
        uint age;
        uint id;
    }

    Member[] public members;

    function _createMember(string _name, uint _age, uint _id) private {
        members.push(Member(name, age));
    }

    function _generateRandomId(string _name) private view returns (uint) {
        return uint(keccak256(_str)) % (idLength ** 10);
    }

    function addMember(string _name, uint _age) public {
        uint id = _generateRandomId(name);
        _createMember(name, age, id);
    }
}
```

### イベント
Solidityには、外部に通知を送る仕組みが用意されている。

```js
contract MyContract {
    uint idLength = 12;

    event AddMember(string name, uint age, uint id);

    struct Member {
        string name;
        uint age;
        uint id;
    }

    Member[] public members;

    function _createMember(string _name, uint _age, uint _id) private {
        members.push(Member(name, age));
        AddMember(_name, _age, _id);
    }

    function _generateRandomId(string _name) private view returns (uint) {
        return uint(keccak256(_str)) % (idLength ** 10);
    }

    function addMember(string _name, uint _age) public {
        uint id = _generateRandomId(name);
        _createMember(name, age, id);
    }
}

```

以下のようにすることでイベントを受け取ることができる
```js
MyContract.AddMember(function(error, result) {

}
```



