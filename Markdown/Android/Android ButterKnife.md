#ButterKnife
ButterKnifeを使うと findViewById でレイアウトのIDとメンバ変数の連結と、set~EventLisntener系のイベント処理の設定が簡潔に書けるようになる。

|リンク|説明|
|---|---|
|[Butter Knifeの紹介](http://qiita.com/yyaammaa/items/cb52ad37309c2e195e56)|Butter Knifeの使用例|
|[[Android]ButterKnife の View Injection コードを一発自動生成するプラグイン『ButterKnifeZelezny』](http://dev.classmethod.jp/smartphone/android-butterknifezelezny/)|Android Studioで Butter Kinife用のコードを自動で出力してくれるプラグイン|

##導入

###Gradleにライブラリの使用を追加
build.gradle(Module: app)のdependenciesに以下を記述する。

`compile 'com.jakewharton:butterknife:6.1.0'`


###ButterKnifeZeleznyプラグインの追加
Android Studioにプラグインの追加

[[Android]ButterKnife の View Injection コードを一発自動生成するプラグイン『ButterKnifeZelezny』](http://dev.classmethod.jp/smartphone/android-butterknifezelezny/)


##使用方法
###Activity

```java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = "myLog";

    // @InjectView で メンバ変数に割り当て
    @InjectView(R.id.button)
    Button button;
    @InjectView(R.id.button2)
    Button button2;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // injectする
        ButterKnife.inject(this);
    }
 
    // イベント処理をまとめる   
    @OnClick({R.id.button, R.id.button2})
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.button:
                break;
            case R.id.button2:
                break;
        }
    }
}

```

###Fragment
Fragmentクラスで使う場合は以下の手順

* onCreateViewでButterKnife.inject
* onDestroyで ButterKnife.reset
* InjectViewでViewとメンバ変数の結びつけ
* onClick等のイベントメソッドを定義

```java
import butterknife.ButterKnife;
import butterknife.InjectView;
import butterknife.OnClick;

public class Fragment1 extends Fragment{
    public static final String FRAMGMENT_NAME = Fragment1.class.getName();

    @InjectView(R.id.button)
    Button button;
    @InjectView(R.id.textView)
    TextView textView;

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment1, container, false);
        ButterKnife.inject(this, view);
        textView.append("hogehogehoge\n");

        return view;
    }

@Override
public void onDestroy() {
    super.onDestroy();
    // onDestroyでresetする
    ButterKnife.reset(this);
}

@OnClick({R.id.button})
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.button:
                textView.append("button\n");
                break;
        }
    }
}
```