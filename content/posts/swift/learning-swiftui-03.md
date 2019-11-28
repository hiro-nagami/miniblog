---
title: "Learning Swiftui 03"
date: 2019-11-27T02:06:27+09:00
draft: false
tags: [ "ios", "swiftui", "swift" ]
---

## データモデルをビューに流し込み表示
### リスト化ようのModelとRowを作る

参考にするのは、[こちら](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
リストにデータを流し込むためのモデルを作る

```
struct Data {
    let name: String
    let image: Image
}
```

Rowビューをつくる

```
struct Row: View {
    var data: Data

    var body: some View {
        data.image
                .resizable()
                .frame(width: 50, height: 50)
        Text(landmark.name)
    }
}
```


Rowビューにデータモデルを流し込んで表示する

```
struct Row_Previews: PreviewProvider {
    static var previews: some View {
        Row(landmark: datalist[0])
    }
}
```

### プレビューで複数のRowを表示
プレビューを複数表示するにはGroupを使う
GroupはContainer Viewというものに含まれる

https://developer.apple.com/documentation/swiftui/view_layout_and_presentation

```
struct Row_Previews: PreviewProvider {
    static var previews: some View {
        Group {
            Row(landmark: datalist[0])
                .previewLayout(.fixed(width: 300, height: 70))
            Row(landmark: datalist[1])
                .previewLayout(.fixed(width: 300, height: 70))
        }
    }
}
```

以下の書き方もできる

```
struct Row_Previews: PreviewProvider {
    static var previews: some View {
        Group {
            Row(landmark: datalist[0])
            Row(landmark: datalist[1])
        }
        .previewLayout(.fixed(width: 300, height: 70))
    }
}
```

## データリストをリスト表示する
### リスト化するビューを作る

```
struct RowList: View {
    var datas: Data[]

    var body: some View {
        List(datas, id: \.id) { (data) in
            Row(data)
        }
    }
}
```

DataがIdentifiableならidプロパティを省略できる。
この`\.id`はKeyPathのexpressionの１つ。

https://stackoverflow.com/questions/52543502/swift-what-does-backslash-dot-mean
https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-expression

```
struct RowList: View {
    var datas: Data[]

    var body: some View {
        NavigationView {
            List(datas) { (data) in
                Row(data)
            }
        }
    }
}
```

## リストをプレビューで表示
```
struct Row_Previews: PreviewProvider {
    static var previews: some View {
        RowList(datas)
    }
}
```

## プッシュで画面を遷移する

### 詳細画面の作成
```
struct DetailView: View {
    var data: Data
    var body: some View {
        HStack {
            Text("\(data.id)")
            Text(data.name)
        }
    }
}
```

### 詳細画面への繊維
```
struct ListSampleView: View {
    var datas: [Data] = [
        Data(id: 1, name: "Taro"),
        Data(id: 2, name: "Hanako")
    ]

    var body: some View {
        NavigationView {
            List(datas, id: \.id) { (data) in
                NavigationLink(destination: DetailView(data: data)) {
                    Row(data: data)
                }
            }
        }
    }
}
```


## SwiftUIを使った初期ビュー
RootとなるViewはSceneDelegateを通して指定する。AppDelegateで何かを指定している。

```
    // MARK: UISceneSession Lifecycle
    func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        // Called when a new scene session is being created.
        // Use this method to select a configuration to create the new scene with.
        return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
    }

    func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
        // Called when the user discards a scene session.
        // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
        // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
    }
```


## ソースコード
https://github.com/hiro-nagami/swiftui-sample/tree/b9dac5327d12196b19133e2569b1939cba809acd