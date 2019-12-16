---
title: "Learning Swift Protocol"
date: 2019-12-17T08:08:13+09:00
tags: [ "ios", "protocol", "swift" ]
draft: false
---

## Protocolのテクニック

```swift
protocol Noticeable {
    func getNotice()
    func openNotice(url: NSURL)
    func closeNotice()
}

extension Noticeable where Self: UITableViewController {
    func getNotice() {
        NoticeAPIClient.find()
        .success { (notice: Notice) -> Void in
            let noticeView = NoticeView.view()
            noticeView.notice = notice
            self.tableView.tableHeaderView = noticeView
        }
    }

    func openNotice(url: NSURL) {
        let webVC = //WebViewでお知らせを表示
        self.navigationController?.presentViewController(webVC, animated: true, completion: nil)
    }

    func closeNotice() {
        guard let noticeView = self.tableView.tableHeaderView as? NoticeView else { return }

        self.tableView.tableHeaderView = nil
    }
}
```


```swift

extension UITableView: XibBuilerCompatible {}

public extension XibBuilerExtension where Base: UITableView {
    public func register(cellClass: XibBuilderProtocol.Type) {
        let nib = UINib(nibName: cellClass.xibName, bundle: nil)
        self.base.register(nib, forCellReuseIdentifier: cellClass.xibName)
    }

    public func register(cellClasses: [XibBuilderProtocol.Type]) {
        for clazz in cellClasses {
            let nib = UINib(nibName: clazz.xibName, bundle: nil)
            self.base.register(nib, forCellReuseIdentifier: clazz.xibName)
        }
    }

    public func dequeueReusableCell<T: XibBuilderProtocol>(clazz: T.Type) -> T {
        return self.base.dequeueReusableCell(withIdentifier: T.xibName) as! T
    }
}

public protocol XibBuilderProtocol {
    static var xibName: String { get }
}

extension XibBuilderProtocol {
    public static func getModule() -> Self {
        let nib = UINib(nibName: Self.xibName, bundle: nil)
        guard let module = nib.instantiate(withOwner: self, options: nil).first as? Self else {
            fatalError("Cannot find module has xibName \(Self.xibName)")
        }

        return module
    }
}

public final class XibBuilerExtension<Base> {
    public let base: Base
    public init(_ base: Base) {
        self.base = base
    }
}

public protocol XibBuilerCompatible {
    associatedtype CompatibleType
    var sb: CompatibleType { get }
}

public extension XibBuilerCompatible {
    public var sb: XibBuilerExtension<Self> {
        return XibBuilerExtension(self)
    }
}
```

## 参考
- https://qiita.com/sagaraya/items/5e73501ff7a3f0c502c0