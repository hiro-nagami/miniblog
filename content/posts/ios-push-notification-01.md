---
title: "iOS Push Notification 01"
date: 2019-10-29T13:39:45+09:00
draft: false
tags: [ "ios", "notification" ]
---

## iOSのプッシュ通知について
iOS プッシュ通知の実装方法は大きく分けて３つある。
- AppDelegateメソッドを使う方法
- UserNotificationCenterDelegateメソッドを使う方法 (iOS10~)
- NotificationServiceExtensionを使う方法

## AppDelegateを使う方法
AppDelegateには、プッシュ通知を受け取る以下のメソッドが用意されている
https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623013-application

### 実装方法
```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any],
                    fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    // アプリがバックグラウンド状態の時に通知を受け取った場合の処理を行う。
    completionHandler(UIBackgroundFetchResult.newData)
}
```

## UserNotificationCenterを使う方法
UserNotificationCenterDelegateすることで、以下のメソッドでプッシュ通知を受け取ることができる。
https://developer.apple.com/documentation/usernotifications/unusernotificationcenterdelegate/1649518-usernotificationcenter

```swift
func userNotificationCenter(_ center: UNUserNotificationCenter,
                            willPresent notification: UNNotification,
                            withCompletionHandler completionHandler: (UNNotificationPresentationOptions) -> Void) {
    // Remote Notification
    if notification.request.trigger is UNPushNotificationTrigger {
        debugPrint("プッシュ通知受信")
        completionHandler([.sound, .alert])
    } else {
        // ...
    }
}

func userNotificationCenter(_ center: UNUserNotificationCenter,
                            didReceive response: UNNotificationResponse,
                            withCompletionHandler completionHandler: () -> Void) {
    debugPrint("opened")
    completionHandler()
}
```

## NotificationServiceExtensionを使う方法
XcodeプロジェクトからNotificationServiceExtensionを追加することでリッチ通知を実装することができる。
https://developer.apple.com/documentation/usernotificationsui/customizing_the_appearance_of_notifications

追加をすると自動的に`NotificationService.swift`が追加され、そこに受信ロジックを追加することになる。

### 実装方法
```swift
pulic class NotificationService: UNNotificationServiceExtension {
    override func didReceive(_ request: UNNotificationRequest, withContentHandler contentHandler:(UNNotificationContent) -> Void) {
    }
}
```