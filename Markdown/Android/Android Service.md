#サービス Service
Serviceは通常のアプリと異なりメインのUIを持たずにバックグラウンドで動くアプリ。
SerivceクラスにはServiceとIntentServiceがある。
Serviceは呼び出し元のスレッドと同じスレッドで実行されるため、処理が終わるまで呼び出し元の処理を占有する。
IntentServiceは別スレッドを自動で作成し、そちらのスレッドで処理するため呼び出し元のスレッドは占有されない。


AndroidManifest.xml に以下を追加

```xml
  <service android:name="サービスクラス名" />
</application>
```

###Service
ただのService。
Serviceの処理が終わるまでは呼び出し元の処理が固まる。Serviceで時間のかかる処理を行う場合はAsyncTask等で別スレッドで呼び出すようにする。



```java
- MyService.java -

public class MyService extends Service {

    final static String TAG = "MyService";

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

        try {
            // 時間のかかる処理を行うと呼び出し元のスレッドが固まる
            for (int i=0; i<3; i++) {
                Thread.sleep(1000);
                Log.d(TAG, "sleep " + i);
            }
        } catch (InterruptedException e) {
        }

        return START_STICKY;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d(TAG, "onDestroy");
    }

}
```

```java
- TestServiceActivity.java -
MyServiceを呼び出す

public class TestServiceActivity extends AppCompatActivity implements OnClickListener {
    private Button[] buttons = new Button[3];
    private int[] button_ids = new int[]{
            R.id.button,
            R.id.button2,
            R.id.button3 };

    TextView mTextView;
    MyBindService mService;
    MyBindService.BindServiceBinder mBinder;
    boolean         mBound;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        for (int i=0; i<buttons.length; i++) {
            buttons[i] = (Button)findViewById(button_ids[i]);
            buttons[i].setOnClickListener(this);
            buttons[i].setText("test" + String.valueOf(i+1));
        }

        mTextView = (TextView)findViewById(R.id.textView);
    }

    public void onClick(View v) {
        switch(v.getId()) {
            case R.id.button:
                test1();
                break;
            case R.id.button2:
                test2();
                break;
            case R.id.button3:
                test3();
                break;
        }
    }

    // コネクション作成
    private ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName className, IBinder service) {
            // サービス接続時に呼ばれる
            Log.i("ServiceConnection", "onServiceConnected");
            // バインダーを保存
            MyBindService.BindServiceBinder binder = (MyBindService.BindServiceBinder) service;
            mService = binder.getService();
        }
        @Override
        public void onServiceDisconnected(ComponentName arg0) {
            // サービス切断時に呼ばれる
            Log.i("ServiceConnection", "onServiceDisconnected");
            mService = null;
        }
    };

    private void test1() {
        // バインド開始
        boolean ret = bindService( new Intent( MainActivity.this, MyBindService.class ) ,
                connection,
                Context.BIND_AUTO_CREATE
        );
        if (ret) {
            mBound = true;
            mTextView.append("bindService 成功\n");
        } else {
            mTextView.append("bindService 失敗\n");
        }
    }
    private void test2() {
        if( mBound ) {
            // バインドされている場合、バインドを解除
            unbindService(connection);
            mBound = false;
            mTextView.append("bind解除\n");
        } else {
            mTextView.append("bindされていません\n");
        }
    }
    private void test3() {
        if( mBound ){
            // 適当な関数を呼び出し
            String msg = mService.TestFunction();
            mTextView.append(msg);
        } else {
            mTextView.append("bindされていません\n");
        }
    }
}
```

###bindSerivce
通常のサービスは呼び出し元のプロセス（アプリ)とは独立して動作するので、アプリが終了しても動き続けるが、バインドされたサービスはアプリが終了するとサービスも一緒に終了する。

[バインドされたサービス](https://developer.android.com/guide/components/bound-services.html?hl=ja)

bindするサービスの作り方

```java
- LocalService.java -

public class LocalService extends Service {
    // Binder given to clients
    private final IBinder mBinder = new LocalBinder();
    // Random number generator
    private final Random mGenerator = new Random();

    /**
     * Class used for the client Binder.  Because we know this service always
     * runs in the same process as its clients, we don't need to deal with IPC.
     */
    public class LocalBinder extends Binder {
        LocalService getService() {
            // Return this instance of LocalService so clients can call public methods
            return LocalService.this;
        }
    }

    @Override
    public IBinder onBind(Intent intent) {
        return mBinder;
    }

    /** method for clients */
    public int getRandomNumber() {
      return mGenerator.nextInt(100);
    }
}
```

```java
- BindingActivity.java -

public class BindingActivity extends Activity {
    LocalService mService;
    boolean mBound = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
    }

    @Override
    protected void onStart() {
        super.onStart();
        // Bind to LocalService
        Intent intent = new Intent(this, LocalService.class);
        bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
    }

    @Override
    protected void onStop() {
        super.onStop();
        // Unbind from the service
        if (mBound) {
            unbindService(mConnection);
            mBound = false;
        }
    }

    public void onButtonClick(View v) {
        if (mBound) {
            int num = mService.getRandomNumber();
            Toast.makeText(this, "number: " + num, Toast.LENGTH_SHORT).show();
        }
    }

    private ServiceConnection mConnection = new ServiceConnection() {

        @Override
        public void onServiceConnected(ComponentName className,
                IBinder service) {
            LocalBinder binder = (LocalBinder) service;
            mService = binder.getService();
            mBound = true;
        }

        @Override
        public void onServiceDisconnected(ComponentName arg0) {
            mBound = false;
        }
    };
}
```

###IntentService
IntentServiceは呼び出し元のスレッドをブロックしない。

特徴

* 別スレッドを作成
* 仕事のキューイング
* 順次とりだして実行する

```java
- MyIntentService.java -

public class MyIntentService extends IntentService{
    final static String TAG = "MyService";

    public MyIntentService() {
        super("MyIntentService");
    }

    /**
     * サービスが立ち上がると自動で実行される処理
     * @param intent
     */
    @Override
    protected void onHandleIntent(Intent intent) {
        try {
            // 時間のかかる処理を行うと呼び出し元のスレッドが固まる
            for (int i=0; i<3; i++) {
                Thread.sleep(1000);
                Log.d(TAG, "sleep " + i);
            }
        } catch (InterruptedException e) {
        }
    }
}

- TestIntentActivity.java -

public class TestIntentActivity extends AppCompatActivity implements OnClickListener{
    TextView mTextView;

    private Button[] buttons = new Button[2];
    private int[] button_ids = new int[]{
            R.id.button,
            R.id.button2};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_test_intent);

        for (int i = 0; i < buttons.length; i++) {
            buttons[i] = (Button) findViewById(button_ids[i]);
            buttons[i].setOnClickListener(this);
            buttons[i].setText("test" + String.valueOf(i + 1));
        }

        mTextView = (TextView)findViewById(R.id.textView2);
    }

    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button:
                test1();
                break;
            case R.id.button2:
                test2();
                break;
        }
    }

    // サービス開始
    private void test1() {
        mTextView.append("Start Service\n");
        startService(new Intent(getBaseContext(), MyIntentService.class));
    }

    // サービス停止
    private void test2() {
        mTextView.append("Stop Service\n");
        stopService(new Intent(getBaseContext(), MyIntentService.class));
    }
}

```