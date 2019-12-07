---
title: "Learning Swiftui 06"
date: 2019-12-07T13:05:57+09:00
draft: false
tags: [ "ios", "swiftui", "swift" ]
---

## `@EnvironmentObject`について
`.environmentObject(env)` 修飾子を呼び出したプロパティでは `@EnvironmentObject` を適用したプロパティに自動的に値を取得するようになる。Viewの階層の中で、Initで引数を渡すことなく参照することができるようになる。 `dynamic member lookup` の機能によりObservableObjectを見つけてくれる。

```

struct ToggleSampleView: View {
    @EnvironmentObject var toggleSample: ToggleSampleModel

    var body: some View {
        List {
            Toggle(isOn: $toggleSample.isToggleOn) {
                Text("sample")
            }

            if (toggleSample.isToggleOn) {
                Circle().foregroundColor(.blue)
                Circle().foregroundColor(.blue)
            }
        }
    }
}

struct ToggleSampleView_preview: PreviewProvider {
    static var previews: some View {
        ToggleSampleView().environmentObject(ToggleSampleModel())
    }
}


```