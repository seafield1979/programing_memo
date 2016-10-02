#ヴィジット Widgets


基本
IDを元にWidgetを取得する

```java
// コンテントのWidgetを取得する
// Widgetのクラス 変数 = (Widgetのクラス)findViewById(R.id.<WidgetのID>);
Button button2 = (Button)findViewById(R.id.button2);
```

###Plain TextView
表示専用(入力不可能)なテキスト

```java
// 値を設定する
TextView textView = (TextView)findViewById(R.id.textView1);
textView.setText("hoge");

// 値を取得する
String str = textView.getText().toString();
System.out.println(str);
```

###Toast
少しの間表示されて消えるポップアップのようなもの

```java
import android.widget.Toast;

public class MainActivity extends AppCompatActivity{
  
  @Override
  protected void onCreate(Bundle savedInstanceState) {
      ~~~
    toastMake("hoge", 0, 0);
  }
  // Toast を表示する
  private void toastMake(String message, int x, int y){
      Toast toast = Toast.makeText(this, message, Toast.LENGTH_LONG);
      toast.setGravity(Gravity.CENTER | Gravity.CENTER, x, y);
      toast.show();
  }
}
```

###Text
入力可能なテキスト

###Button


###RadioButton

###CheckBox

###Switch

###ToggleButton


###ImageButton

###ImageView

###Progress Bar

###Seek Bar

###Rating Bar

###Spinner

###WebView