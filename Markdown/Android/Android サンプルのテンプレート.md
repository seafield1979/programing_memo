#テンプレート template
サンプルの量産化を進めるためによく使うレイアウトや処理のテンプレートをまとめてみた。

###テストのメニュー画面
各画面のActivityを呼び出すボタンがたくさんある。

![](http://sunsunsoft.com/image/android/template_buttons.png)

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.example.shutaro.testscroll.MenuActivity">

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

</LinearLayout>
```

```java
MenuActivity.java

import android.view.View.OnClickListener;

public class MenuActivity extends AppCompatActivity implements OnClickListener {
    private Button[] buttons = new Button[6];
    private int[] button_ids = new int[]{
                R.id.button,
                R.id.button2,
                R.id.button3,
                R.id.button4,
                R.id.button5,
                R.id.button6 };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);

        for (int i=0; i<buttons.length; i++) {
            buttons[i] = (Button)findViewById(button_ids[i]);
            buttons[i].setOnClickListener(this);
            buttons[i].setText("test" + String.valueOf(i+1));
        }
    }

    public void onClick(View v) {
        switch(v.getId()) {
            case R.id.button: {
                Log.v("myLog", "button was pushed");
                Intent i = new Intent(getApplicationContext(),MainActivity.class);
                startActivity(i);
            }
                break;
            case R.id.button2:
                Log.v("myLog", "button2 was pushed");
                break;
            case R.id.button3:
                Log.v("myLog", "button3 was pushed");
                break;
            case R.id.button4:
                Log.v("myLog", "button4 was pushed");
                break;
            case R.id.button5:
                Log.v("myLog", "button5 was pushed");
                break;
            case R.id.button6:
                Log.v("myLog", "button6 was pushed");
                break;
        }
    }
}

```