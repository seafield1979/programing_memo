#ボタン Button

###アプリからレイアウトのボタンを参照
(Button)findViewById(R.id.ボタンのid名) でボタンのオブジェクトを参照できる。

```java
import android.widget.Button;
import android.view.View.OnClickListener;

public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    
        // ボタンのタイトルを変更
        Button button1 = (Button)findViewById(R.id.button1);
        button1.setText("hoge");
    }
}
```

###ボタンを押した時に何かしら処理を行う
ボタンが押された時の処理を実装するにはButtonオブジェクトに setOnClickListener で押されたときに処理が呼ばれるようにする。

手順は以下

* content*.xml で Buttonを配置する。idを設定する(例:button1)
* Activity.java で 
    + android.widget.Button をインポート
    + button1 のオブジェクトを取得(findViewById)
    + button1 に setOnClickListener で クリックした時の処理を追加する

ボタンが押された時の処理を OnClickerLisntener を newするパターンと、アクティビティーのonClickメソッドが呼ばれるパターンで実装

```java
import android.widget.Button;
import android.view.View.OnClickListener;

public class MainActivity extends AppCompatActivity implements OnClickListener {

    private Button button2;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    
        // ボタンを押した時の処理を追加
        Button button1 = (Button)findViewById(R.id.button1);
        button1.setOnClickListener(new OnClickListener() {
            public void onClick(View v) {
                // クリック時の処理
                System.out.println("ボタン押された");
            }
        });
        
        // ボタン２にイベントリスナーを追加
        button2 = (Button)findViewById(R.id.button2);
        button2.setOnClickListener(this);
    }
    
    public void onClick(View v) {
        if (v == button2) {
            Log.v("myLog", "button2 was pushed");
        }
    }
}
```

###ボタンをコードで生成する
レイアウトで配置したボタンではなくプログラム実行時にボタンを生成する方法。以下の例ではメインのActivity の RelativeLayout にボタンを追加する。

```java
-- onCreate あたりで -- 

// コードでボタンを追加
Button button2 = new Button(this);
button2.setText("ぼたん");
button2.setOnClickListener(this);

// レイアウトを追加(ボタン１の下に配置)
RelativeLayout.LayoutParams layout = new RelativeLayout.LayoutParams(
        RelativeLayout.LayoutParams.WRAP_CONTENT,
        RelativeLayout.LayoutParams.WRAP_CONTENT);
layout.addRule(RelativeLayout.BELOW, R.id.button1);
layout.addRule(RelativeLayout.CENTER_HORIZONTAL);
button2.setLayoutParams(layout);

// アクティビティ以下のViewGroup(ここではRelativeRayout)に追加
RelativeLayout relativeLayout = (RelativeLayout)findViewById(R.id.relativeLayout);
relativeLayout.addView(button2);
```