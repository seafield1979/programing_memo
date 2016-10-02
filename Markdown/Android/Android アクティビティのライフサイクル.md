# Activityのライフサイクル lifecycle

![](http://www.javadrive.jp/android/activity/img/p2-1.png)


[Activityのライフサイクル Java-Drive](http://www.javadrive.jp/android/activity/index2.html)


ライフサイクルを確認するサンプル

```java
import android.util.Log;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

    @Override
    public void onStart(){
        super.onStart();
        Log.v("LifeCycle", "onStart");
    }
    
    @Override
    public void onResume(){
        super.onResume();
        Log.v("LifeCycle", "onResume");
    }
    
    @Override
    public void onPause(){
        super.onPause();
        Log.v("LifeCycle", "onPause");
    }
    
    @Override
    public void onRestart(){
        super.onRestart();
        Log.v("LifeCycle", "onRestart");
    }
    
    @Override
    public void onStop(){
        super.onStop();
        Log.v("LifeCycle", "onStop");
    }
    
    @Override
    public void onDestroy(){
        super.onDestroy();
        Log.v("LifeCycle", "onDestroy");
    }
}

```

以下のようなログが出力される

```sh
09-27 16:22:52.672 2021-2021/com.example.shutaro.test1 V/LifeCycle: onStart
09-27 16:22:52.673 2021-2021/com.example.shutaro.test1 V/LifeCycle: onResume
09-27 16:22:52.718 2021-2021/com.example.shutaro.test1 I/Timeline: Timeline: Activity_idle id: android.os.BinderProxy@50672ed time:533644184
09-27 16:22:54.600 2021-2021/com.example.shutaro.test1 V/LifeCycle: onPause
09-27 16:22:54.642 2021-2021/com.example.shutaro.test1 W/IInputConnectionWrapper: showStatusIcon on inactive InputConnection
09-27 16:22:54.976 2021-2021/com.example.shutaro.test1 V/LifeCycle: onStop
09-27 16:22:54.976 2021-2021/com.example.shutaro.test1 V/LifeCycle: onDestroy
```

###起動までのライフサイクル

![](http://www.javadrive.jp/android/activity/img/p2-3.png)

###別のアクティビティが表示されるまでのライフサイクル

![](http://www.javadrive.jp/android/activity/img/p2-5.png)

###停止、再び表示されるまでのライフサイクル
![](http://www.javadrive.jp/android/activity/img/p2-6.png)