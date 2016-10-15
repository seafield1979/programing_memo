#AlarmManager 指定時間に処理を行う
AlarmManagerは日時指定や、一定間隔で処理を行いたい場合に使用する。

AlarmManagerでできること

* 指定の日時に処理(Activity)を行う
* 一定時間毎に処理(Activity)を行う


マニフェストファイルにreceiverを追加する

```xml
- AndroidManifest.xml - 
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.shutaro.testnotification">

    <application
      ・
      ・
      ・
        <receiver
            android:name=".AlarmBroadcastReceiver"
        />
    </application>
```



```java
- AlarmActivity.java - 
public class AlarmActivity extends AppCompatActivity {
  public void test1() {
    // Calendarクラスで日時を指定して処理を行う
    Calendar calendar = Calendar.getInstance(TimeZone.getTimeZone("Asia/Tokyo"), Locale.JAPAN);
    // 現在時刻の５秒後
    calendar.add(Calendar.SECOND, 5);
    
    Intent intent = new Intent(getApplicationContext(), AlarmBroadcastReceiver.class);
    intent.putExtra("intentId", 2);
    PendingIntent pending = PendingIntent.getBroadcast(getApplicationContext(), bid2, intent, 0);
    
    // アラームをセットする
    AlarmManager am = (AlarmManager)AlarmActivity.this.getSystemService(ALARM_SERVICE);
    am.set(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), pending);
  }
  
  // 一定間隔で処理を行う
  public void test2() {
    Calendar calendar = Calendar.getInstance();
    calendar.setTimeInMillis(System.currentTimeMillis());
    // 初回は5秒後に設定
    calendar.add(Calendar.SECOND, 5);

    Intent intent = new Intent(getApplicationContext(), AlarmBroadcastReceiver.class);
    intent.putExtra("intentId", 1);
    // PendingIntentが同じ物の場合は上書きされてしまうので requestCode で区別する
    PendingIntent pending = PendingIntent.getBroadcast(getApplicationContext(), bid1, intent, 0);

    // アラームをセットする
    AlarmManager am = (AlarmManager) AlarmActivity.this.getSystemService(ALARM_SERVICE);
    // 約10秒で 繰り返し
    am.setInexactRepeating(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), 10 * 1000, pending);

    //アプリを一旦終了
    close();
  }
}

- AlartBroadcatReceiver.java - 

public class AlarmBroadcastReceiver extends BroadcastReceiver {
    Context context;

    @Override   // データを受信した
    public void onReceive(Context context, Intent intent) {

        this.context = context;

        Toast.makeText(context, "called ReceivedActivity", Toast.LENGTH_SHORT).show();

        Log.d("AlarmBroadcastReceiver","onReceive() pid=" + android.os.Process.myPid());

        int bid = intent.getIntExtra("intentId",0);

        Intent intent2 = new Intent(context, AlarmActivity.class);
        PendingIntent pendingIntent =
                PendingIntent.getActivity(context, bid, intent2, PendingIntent.FLAG_UPDATE_CURRENT);

        NotificationManager notificationManager =
                (NotificationManager)context.getSystemService(Context.NOTIFICATION_SERVICE);
        Notification notification = new NotificationCompat.Builder(context)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setTicker("時間です")
                .setWhen(System.currentTimeMillis())
                .setContentTitle("TestAlarm "+bid)
                .setContentText("時間になりました")
                // 音、バイブレート、LEDで通知
                .setDefaults(Notification.DEFAULT_ALL)
                // 通知をタップした時にMainActivityを立ち上げる
                .setContentIntent(pendingIntent)
                .build();

        // 古い通知を削除
        notificationManager.cancelAll();
        // 通知
        notificationManager.notify(R.string.app_name, notification);
    }
}
```