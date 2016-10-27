#Hello World

デフォルトのプロジェクトにいろいろ変更を加えてみる。

**hello worldの文字列を変更**
デフォルトで配置された"Hello World"のラベルをダブルクリックして新しいテキストを入力する。

**テキスト(TextView)を追加**
activity.xmlをレイアウト表示時にエディタウィンドウの左側に表示されるPaletteから追加したいWidgetをレイアウトにドラッグ＆ドロップすると追加できる。

![](http://sunsunsoft.com/image/android/add_widget.png)


表示するテキストはWidget(ここではTextView)をダブクルクリックして表示されるポップアップから設定できる。
![](http://sunsunsoft.com/image/android/set_text_id.png)

Widget(ここではTextView)をクリックしてから properties からも text や id を設定できる。
![](http://sunsunsoft.com/image/android/set_properties.png)


**テキストの表示を変更**

プログラム実行時にテキストの表示を変更してみる。

```java
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // テキストを変更する処理
        TextView textView = (TextView)findViewById(R.id.textView);
        textView.setText("hoge!");
    }
}
```

**ボタンを押してみる**

activity_~.xml に Button を配置。idに適当な名前(button1等)を設定する。その後Activityクラスに以下の処理を追加。

```java
import android.widget.Button;
import android.view.View.OnClickListener;

public class MainActivity extends AppCompatActivity {

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
    }
}
```

###トースト(ポップアップのようなもの)

```java
import android.widget.Toast;

public class MainActivity extends AppCompatActivity{
  
  @Override
  protected void onCreate(Bundle savedInstanceState) {
      ~~~
    toastMake("hoge", 0, 0);
  }
  // Toast を表示する
  // x,y はデフォルトの表示位置(画面中央)からのオフセット
  private void toastMake(String message, int x, int y){
      Toast toast = Toast.makeText(this, message, Toast.LENGTH_LONG);
      toast.setGravity(Gravity.CENTER | Gravity.CENTER, x, y);
      toast.show();
  }
}
```

