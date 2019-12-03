---
title: "Learning Swiftui 04"
date: 2019-11-13T19:05:56+09:00
draft: false
tags: [ "ios", "swiftui", "swift" ]
---

## Imageに色をOvarlayする方法

`.foregroundColor()`を使って指定できる。

```
 Image(systemName: "star.fill")
    .foregroundColor(.yellow)
```

## Imageのサイズを指定する
サイズの指定にはが使える`.imageScale()`

```
 Image(systemName: "star.fill")
    .imageScale(.medium)
```

## Filterする方法

ForEachについて
Listの中身を部分的に動的に変更する場合は、ForEachなどをつかう。また、リストを切り替えるためにToggleを使うことができる。

```
List {
    Toggle(isOn: $showFavoritesOnly) {
        Text("Favorites only")
    }

    ForEach(landmarkData) { landmark in
        if !self.showFavoritesOnly || landmark.isFavorite {
            NavigationLink(destination: LandmarkDetail(landmark: landmark)) {
                LandmarkRow(landmark: landmark)
            }
        }
    }
}
```

## @State プロパティは何か
ReactやVueで扱うStateのようなもの？SwiftUIでは、ステートとUIを同期的に処理するために使う。