#テンプレート template
サンプルの量産化を進めるためによく使うレイアウトや処理のテンプレートをまとめてみた。


###ButterKnife
build.gradle(Module: app)のdependenciesに以下を記述する。

```java
compile 'com.jakewharton:butterknife:6.1.0'
```

###テストのメニュー画面
各画面のActivityを呼び出すボタンがたくさんある。

![](http://sunsunsoft.com/image/android/template_buttons.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        android:orientation="horizontal"
        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="New Button"
            android:id="@+id/button"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="New Button"
            android:id="@+id/button2"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="New Button"
            android:id="@+id/button3"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        android:orientation="horizontal"
        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="New Button"
            android:id="@+id/button4"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="New Button"
            android:id="@+id/button5"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="New Button"
            android:id="@+id/button6"/>
    </LinearLayout>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="New Text"
        android:id="@+id/textView"/>
</LinearLayout>
```

```java
MenuActivity.java

import android.view.View.OnClickListener;

public class MenuActivity extends AppCompatActivity implements OnClickListener {
    private int[] button_ids = new int[]{
                R.id.button,
                R.id.button2,
                R.id.button3,
                R.id.button4,
                R.id.button5,
                R.id.button6 };
    private Button[] buttons = new Button[button_ids.length];

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);

        for (int i=0; i<buttons.length; i++) {
            buttons[i] = (Button)findViewById(button_ids[i]);
            buttons[i].setOnClickListener(this);
            buttons[i].setText("activity" + String.valueOf(i+1));
        }
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
            case R.id.button4:
                test4();
                break;
            case R.id.button5:
                test5();
                break;
            case R.id.button6:
                test6();
                break;
        }
    }
    
    private void test1() {
      Intent i = new Intent(getApplicationContext(),MainActivity.class);
      startActivity(i);
    }
    private void test2() {
    }
    private void test3() {
    }
    private void test4() {
    }
    private void test5() {
    }
    private void test6() {
    }
}

```

###ButterKnifeを使ったバージョン

```java
import butterknife.ButterKnife;
import butterknife.InjectView;
import butterknife.OnClick;

public class MenuActivity extends AppCompatActivity {

    @InjectView(R.id.button)
    Button button;
    @InjectView(R.id.button2)
    Button button2;
    @InjectView(R.id.button3)
    Button button3;
    @InjectView(R.id.button4)
    Button button4;
    @InjectView(R.id.button5)
    Button button5;
    @InjectView(R.id.button6)
    Button button6;
    @InjectView(R.id.textView)
    TextView textView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);
        ButterKnife.inject(this);
    }

    @OnClick({R.id.button, R.id.button2, R.id.button3, R.id.button4, R.id.button5, R.id.button6})
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.button:
                test1();
                break;
            case R.id.button2:
                test2();
                break;
            case R.id.button3:
                test3();
                break;
            case R.id.button4:
                test4();
                break;
            case R.id.button5:
                test5();
                break;
            case R.id.button6:
                test6();
                break;
        }
    }
    private void test1() {
      Intent i = new Intent(getApplicationContext(),MainActivity.class);
      startActivity(i);
    }
    private void test2() {

    }
    private void test3() {

    }
    private void test4() {

    }
    private void test5() {

    }
    private void test6() {

    }
}

```

###大量のViewを追加
指定のView以下に大量のViewを追加する。

```java
for (int i=0; i<100; i++) {
    TextView tv = new TextView(this);
    tv.setText(String.format("hoge %d", i));
    if (i%2 == 0) {
        tv.setBackgroundColor(Color.rgb(200,100,0));
    }

    tv.setLayoutParams(new LinearLayout.LayoutParams(
            LinearLayout.LayoutParams.WRAP_CONTENT,
            LinearLayout.LayoutParams.MATCH_PARENT));
    tv.setGravity(Gravity.CENTER);

    linear1.addView(tv);
}
```

###ログ用のListView

![](http://sunsunsoft.com/image/android/widget_logview.png)
ListViewを使用して古いログが流れていくログViewクラスを作成。

```java
public class LogListView extends ListView {
    private static int mLogCount = 0;
    private static boolean mLogReverse = true;
    private static final int LOG_MAX = 10;
    private LinkedList<String> mLinkedList;

    public LogListView(Context context) {
        super(context);
    }
    public LogListView(Context context, AttributeSet attr) {
        super(context, attr);

        mLinkedList = new LinkedList<String>();
    }

    /**
     * ListViewに表示するログを追加する。古いログは自動的に削除される
     * @param message
     */
    public void addLog(String message) {
        String addMsg = String.format("%d %s", mLogCount, message);

        if (mLogReverse) {
            if (mLinkedList.size() < LOG_MAX) {
                mLinkedList.push(addMsg);
            } else {
                mLinkedList.removeLast();
                mLinkedList.push(addMsg);
            }
        } else {
            if (mLinkedList.size() < LOG_MAX) {
                mLinkedList.add(addMsg);
            } else {
                mLinkedList.poll();
                mLinkedList.add(addMsg);
            }
        }

        ArrayAdapter adapter = (ArrayAdapter)this.getAdapter();
        if (adapter != null) {
            adapter.clear();
            for (String str : mLinkedList) {
                adapter.add(str);
            }
        }
        mLogCount++;
    }
}
```