# Notifications (Windows, Linux, macOS)

3 つのオペレーティング システム全て、アプリケーションからの通知をユーザーに送る手段を提供します。 Electron開発者は [HTML5 通知 API](https://notifications.spec.whatwg.org/) の通知を送れて、現在実行中のオペレーティング システムのネイティブ通知 APIs を使用して、表示できます。

**Note:**これは HTML5 API のみレンダラプロセスで利用可能です。 メインプロセスに通知を表示する場合は、[通知](../api/notification.md) モジュールをご覧ください。

```javascript
let myNotification = new Notification('Title', {
  body: 'Lorem Ipsum Dolor Sit Amet'
})

myNotification.onclick = () => {
  console.log('Notification clicked')
}
```

オペレーティング システム コードとユーザー エクスペリエンスは、似ていますが、微妙な違いがあります。

## Windows

* Windows 10 下で, 通知が"うまいこと動く"。
* Windows 8.1 は、Windows 8 のスタート画面に [アプリケーションのユーザー モデル ID](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378459(v=vs.85).aspx) アプリケーションへのショートカットをインストールしなければなりません。 Note, ただし、スタート画面にピン留めする必要はありません。
* On Windows 7, notifications work via a custom implementation which visually resembles the native one on newer systems.

さらに、Windows 8 で通知の本文の最大長は 250 文字、200 文字程度で通知しておくことを Windows チームは推奨しています。 That said, that limitation has been removed in Windows 10, with the Windows team asking developers to be reasonable. Attempting to send gigantic amounts of text to the API (thousands of characters) might result in instability.

### Advanced Notifications

Later versions of Windows allow for advanced notifications, with custom templates, images, and other flexible elements. To send those notifications (from either the main process or the renderer process), use the userland module [electron-windows-notifications](https://github.com/felixrieseberg/electron-windows-notifications), which uses native Node addons to send `ToastNotification` and `TileNotification` objects.

While notifications including buttons work with just `electron-windows-notifications`, handling replies requires the use of [`electron-windows-interactive-notifications`](https://github.com/felixrieseberg/electron-windows-interactive-notifications), which helps with registering the required COM components and calling your Electron app with the entered user data.

### Quiet Hours / Presentation Mode

To detect whether or not you're allowed to send a notification, use the userland module [electron-notification-state](https://github.com/felixrieseberg/electron-notification-state).

This allows you to determine ahead of time whether or not Windows will silently throw the notification away.

## macOS

通知は、すぐに気づくことになるけどmacOS下では、[Apple のヒューマンインターフェイスガイドラインに関する通知](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/NotificationCenter.html)を素直に読んだほうがいい でしょう。

通知サイズが 256 バイトに限定されて、その制限を超えると切り捨てられることに注意してください。

### 高度な通知

macOS以降のバージョンは、ユーザーがすぐに通知に返信できるように、入力フィールドに通知できます。 入力フィールドから通知を送信するためには、ユーザランドモジュール [node-mac-notifier](https://github.com/CharlieHess/node-mac-notifier) を使用します。

### Do not disturb / Session State

To detect whether or not you're allowed to send a notification, use the userland module [electron-notification-state](https://github.com/felixrieseberg/electron-notification-state).

This will allow you to detect ahead of time whether or not the notification will be displayed.

## Linux

Notifications are sent using `libnotify` which can show notifications on any desktop environment that follows [Desktop Notifications Specification](https://developer.gnome.org/notification-spec/), including Cinnamon, Enlightenment, Unity, GNOME, KDE.