---
title: "Learning Swiftui 07"
date: 2019-12-13T08:38:21+09:00
draft: false
tags: [ "ios", "swiftui", "swift" ]
---

## SwiftUIにClean Architectureを採用してみる

### 前提
すでにModelとViewは存在している。ViewにModelをEnvironmentObjectとしてInjectさせている。

モデル: Detail
```
struct Detail: Hashable, Codable, Identifiable {
    var id: Int
    var name: String
    var isLiked: Bool = false
}

```

モデル: DetailList
```
class DetailList: ObservableObject {
    @Published var isLikedOnly: Bool = false
    @Published var details: [Detail] =  [
       Detail(id: 1, name: "Taro"),
       Detail(id: 2, name: "Hanako")
   ]
}
```

ビュー: DetailView
```
struct DetailView: View {
    @EnvironmentObject var detailList: DetailList
    var detail: Detail

    var detailIndex: Int {
        detailList.details.firstIndex(where: { $0.id == detail.id })!
    }

    var body: some View {
        VStack {
            HStack {
                Text("\(detail.id)")
                Text(detail.name)
                Button(action: {
                    self.detailList.details[self.detailIndex].isLiked.toggle()
                }) {
                    if (self.detailList.details[self.detailIndex].isLiked) {
                        Rectangle()
                            .frame(width: 20, height: 20, alignment: .center)
                            .foregroundColor(.yellow)
                    } else {
                        Rectangle()
                            .frame(width: 20, height: 20, alignment: .center)
                            .foregroundColor(.clear)
                            .border(Color.yellow)
                    }
                }
            }
            Spacer()
        }
    }
}
```

### ゴール
ここにPresenterクラスを差し込みたい。

### 課題1
PresenterクラスとモデルのどちらをEnvironmentObjectとするか。
-> 各ビューで変数として持ちたいのはモデルの方
なので、PresenterにモデルをEnvironmentObjectとして持たせてそれをビューが参照する。

### 課題2
PresenterOutputにViewを指定しないとenvironmentObject()修飾子を使って、モデルを受け渡せない
-> PresenterでViewを指定して、プレゼンターにbodyを定義してみる

### 課題3
PresenterOutputProtocolにViewを指定したい。environmentObjectを利用して引数を渡したいため。
-> EnvironmentObjectを使うのはViewに対する依存性が強くなるため

## 実装

DetailPresenter

```

protocol DetailPresenter {
    var detail: Detail { get }
    var detailList: DetailList { get }
    var detailIndex: Int { get }

    func toggleLike()
}

protocol DetailPresenterOutput {
    var presenter: DetailPresenter { get }
}

class DetailPresenterImpl: DetailPresenter {
    @ObservedObject internal var detailList: DetailList
    internal var detail: Detail
    private var view: DetailPresenterOutput?

    var detailIndex: Int {
        self.detailList.details.firstIndex(where: { $0.id == self.detail.id })!
    }

    init(detail: Detail, detailList: DetailList) {
        self.detail = detail
        self.detailList = detailList
    }

    func inject(view: DetailPresenterOutput) {
        self.view = view
    }

    func toggleLike() {
        self.detail.isLiked.toggle()
    }
}
```

DetailView
```

struct DetailView: DetailPresenterOutput, View {
    var presenter: DetailPresenter

    var body: some View {
        VStack {
            HStack {
                Text("\(self.presenter.detail.id)")
                Text(self.presenter.detail.name)
                Button(action: {
                    self.presenter.toggleLike()
                }) {
                    if (self.presenter.detail.isLiked) {
                        Rectangle()
                            .frame(width: 20, height: 20, alignment: .center)
                            .foregroundColor(.yellow)
                    } else {
                        Rectangle()
                            .frame(width: 20, height: 20, alignment: .center)
                            .foregroundColor(.clear)
                            .border(Color.yellow)
                    }
                }
            }
            Spacer()
        }
    }
}

struct DetailView_Previews: PreviewProvider {
    static var previews: some View {
        let detailList = DetailList()
        let detail = detailList.details[0]
        let presenter = DetailPresenterImpl(detail: detail, detailList: detailList)
        let view = DetailView(presenter: presenter)
        presenter.inject(view: view)

        return view
    }
}

```