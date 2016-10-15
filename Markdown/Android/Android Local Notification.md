#Notification お知らせ
ローカルでお知らせ通知を出す方法

シンプルなお知らせ
お知らせ一覧にタイトルだけのシンプルなお知らせを追加するサンプル。お知らせをタップするとこのアプリが立ち上がる。

```java
// ステータスバーを通知

// タップ時に呼び出されるActivity
Intent intent = new Intent(getApplicationContext(),MainActivity.class);
PendingIntent contentIntent = PendingIntent.getActivity(this,
        ID_NOTIFICATION_SAMPLE_ACTIVITY, intent,
        PendingIntent.FLAG_UPDATE_CURRENT);

// Notificationの設定
Notification.Builder nb = new Notification.Builder(this);
// 通知イベントのタイムスタンプ
nb.setWhen(System.currentTimeMillis());
// コンテンツをセット
nb.setContentIntent(contentIntent);
// アイコンをセット
nb.setSmallIcon(android.R.drawable.stat_notify_chat);
// 通知時に表示する文字列
nb.setTicker(getString(R.string.label_status_ticker));
// ステータスバーに表示するタイトル
nb.setContentTitle(getString(R.string.label_launch_browser));
// 音とバイブとライト
nb.setDefaults(Notification.DEFAULT_SOUND
        | Notification.DEFAULT_VIBRATE
        | Notification.DEFAULT_LIGHTS);
// タップすると自動で表示を消す
nb.setAutoCancel(true);
Notification notification = nb.build();

// Notificationを通知
notificationManager.notify(ID_NOTIFICATION_SAMPLE_ACTIVITY,
        notification);
```

###いろいろなレイアウトのお知らせ
![](http://sunsunsoft.com/image/android/notification_type.png)

```java
// ステータスバーを通知
// ブラウザを起動するPendingIntentを生成
Intent intent = new Intent(this, MainActivity.class);
PendingIntent contentIntent = PendingIntent.getActivity(this,
        ID_NOTIFICATION_SAMPLE_ACTIVITY, intent,
        PendingIntent.FLAG_UPDATE_CURRENT);

// Notificationの設定
Notification.Builder nb = new Notification.Builder(this);
nb.setWhen(System.currentTimeMillis());
nb.setContentIntent(contentIntent);
nb.setSmallIcon(android.R.drawable.stat_notify_chat);
nb.setTicker(getString(R.string.label_status_ticker));
nb.setContentTitle(getString(R.string.label_status_style));
nb.setDefaults(Notification.DEFAULT_SOUND
        | Notification.DEFAULT_VIBRATE);

Notification notification = null;
if (type.equals(NotificationType.BigText)) {
    // ビッグテキストのステータスバーを生成
    Notification.BigTextStyle bigTextStyle = new Notification.BigTextStyle(
            nb);
    bigTextStyle.bigText(getString(R.string.label_status_main));
    bigTextStyle.setSummaryText(getString(R.string.label_status_summary));
    notification = bigTextStyle.build();
} else if (type.equals(NotificationType.BigPicture)) {
    // ビッグピクチャーのステータスバーを生成
    Notification.BigPictureStyle bigPictureStyle = new Notification.BigPictureStyle(
            nb);
    bigPictureStyle.bigPicture(BitmapFactory.decodeResource(
            getResources(), R.drawable.ch1302_notification_img));
    notification = bigPictureStyle.build();
} else if (type.equals(NotificationType.Inbox)) {
    // インボックスのステータスバーを生成
    Notification.InboxStyle inboxStyle = new Notification.InboxStyle(
            nb);
    inboxStyle.addLine(getString(R.string.label_status_showline1));
    inboxStyle.addLine(getString(R.string.label_status_showline2));
    inboxStyle.addLine(getString(R.string.label_status_showline3));
    inboxStyle.addLine(getString(R.string.label_status_showline4));
    inboxStyle
            .setSummaryText(getString(R.string.label_status_showlinemore));
    notification = inboxStyle.build();
} else if (type.equals(NotificationType.Button)) {
    // ボタンのステータスバーを生成
    // 電話をかけるIntent
    Intent dialIntent = new Intent(Intent.ACTION_DIAL,
            Uri.parse("tel:090-1234-5678"));
    dialIntent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    PendingIntent dialPending = PendingIntent.getActivity(this,
            ID_NOTIFICATION_SAMPLE_ACTIVITY, dialIntent,
            PendingIntent.FLAG_UPDATE_CURRENT);

    // メールを送るIntent
    Intent smsIntent = new Intent(Intent.ACTION_SENDTO);
    smsIntent.setData(Uri.parse("smsto:090-1234-5678"));
    smsIntent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    PendingIntent smsPending = PendingIntent.getActivity(this,
            ID_NOTIFICATION_SAMPLE_ACTIVITY, smsIntent,
            PendingIntent.FLAG_UPDATE_CURRENT);

    Notification.Action action1 = new Notification.Action.Builder(android.R.drawable.ic_dialog_dialer, getString(R.string.label_status_button_call), dialPending).build();
    Notification.Action action2 = new Notification.Action.Builder(android.R.drawable.ic_dialog_email, getString(R.string.label_status_button_sms), smsPending).build();
    nb.addAction(action1);
    nb.addAction(action2);
    notification = nb.build();
}

// Notificationを通知
if (notification != null) {
    notificationManager.notify(ID_NOTIFICATION_SAMPLE_ACTIVITY,
            notification);
}
```