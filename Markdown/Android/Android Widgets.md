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
  // x,y はデフォルトの表示位置(画面中央)からのオフセット
  private void toastMake(String message, int x, int y){
      Toast toast = Toast.makeText(this, message, Toast.LENGTH_LONG);
      toast.setGravity(Gravity.CENTER | Gravity.CENTER, x, y);
      toast.show();
  }
}
```

###自前のWidgetを作成する

1.既存のWidgetクラスを継承したクラスを作成する
2.1のクラスのコンストラクタをオーバーライドする
3.1のクラスに独自の機能を実装する


* レイアウト画面で子のビューのテキストサイズやレイアウトの変更ができない

```java
package com.example.shutaro.testgesture;

public class LogListView extends ListView {
    public LogListView(Context context) {
        super(context);
    }
    public LogListView(Context context, AttributeSet attr) {
        super(context, attr);

        // ここに初期化処理を書く
    }
}
```

```xml
レイアウトのxml
自前Widgetを使う場合は先頭にパッケージ名をつける

<com.example.shutaro.testgesture.LogListView
        android:layout_width="wrap_content"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:id="@+id/listView2"
        android:layout_centerHorizontal="true"/>
```

###Text
入力可能なテキスト


###Button
[Android ボタン](quiver:///notes/D8C21432-74F7-4F0A-AFB0-A1C147B28EEA)

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