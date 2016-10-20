#Timer タイマー
Androidアプリで一定時間ごとに処理を行いたい場合の手段

リンク

[TECHBOOSTER TIMERを使って定期実行する](http://techbooster.org/android/application/934/)

[Android開発入門 繰り返し処理を行うとき](http://android.keicode.com/basics/services-repeated-task.php)

##Timerクラス
タイマーを使うと一定時間ごとにイベントを発生させることが出来る
###メインスレッドで使う
メインスレッドUIスレッドでタイマーを呼び出す場合でも、タイマー処理は別スレッドなので、そのままメインのUIにアクセスできない。この場合はrunOnUiThread()のrun()内でUIにアクセスする。

```java
public class Test1Activity extends AppCompatActivity 
{
  Timer timer = new Timer();
  final int INTERVAL_PERIOD = 2000;
  
  private void test1() {
    // TimerオブジェクトのscheduleAtFixedRateにTimerTaskオブジェクトを渡す
    timer.scheduleAtFixedRate(new TimerTask(){
        @Override
        public void run() {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    textView.append("Hello!\n");
                }
            });
        }
    }, 0, INTERVAL_PERIOD);
  }
}
```

###Serviceスレッドで使う
Serviceスレッド内でタイマーを使う。Serviceの処理はメインのActivityから独立しているため、UIにアクセスしたりはできない。

```java
public class TimerService extends Service{
    final static String TAG = "MyService";
    final int INTERVAL_PERIOD = 2000;
    Timer timer = new Timer();

    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Log.d(TAG, "onCreate");
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d(TAG, "onStartCommand");

        timer.scheduleAtFixedRate(new TimerTask(){
            @Override
            public void run() {
                Log.d(TAG, "Hello!");
            }
        }, 0, INTERVAL_PERIOD);

        return START_STICKY;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        if(timer != null){
            timer.cancel();
        }
        Log.d(TAG, "onDestroy");
    }
}

- Activity.java - 
//Service呼び出し
startService(new Intent(getBaseContext(), TimerService.class));
//Service停止
stopService(new Intent(getBaseContext(), TimerService.class));
```

##RunnableクラスとThread
Thread無いでsleepする方法。run()メソッド内でsleep()を行い、これがタイマーの役割を果たす。

```java
class MyRunnable implements Runnable {
    final int INTERVAL_PERIOD = 2000;
    private int mCount = 0;
    private boolean running = true;

    public void run() {
        Log.v("myLog", "thread1 start");
        while(running) {
            Log.v("myLog", "count " + mCount);
            mCount++;
            try {
                Thread.sleep(INTERVAL_PERIOD);
            } catch (InterruptedException e) {
            }
        }
        Log.v("myLog", "thread1 stop");
    }

    public void stopRequest() {
        running = false;
    }
}

public class Test1Activity extends AppCompatActivity {
  Thread mThread1 = null;
  MyRunnable mMyRunnable = null;
  
  /**
   * スレッドの開始
   */ 
  private void start() {
      if (mThread1 == null) {
          mMyRunnable = new MyRunnable();
          mThread1 = new Thread(mMyRunnable);
          mThread1.start();
      }
  }

  /**
   * スレッドの停止
   */
  private void stop() {
      if (mThread1 != null) {
          mMyRunnable.stopRequest();
          // 停止を待つ
          try {
              mThread1.join();
          } catch (InterruptedException e) {
              Log.e("myLog", e.toString());
          }
          mThread1 = null;
      }
  }
}
```