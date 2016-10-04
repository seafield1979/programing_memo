#Spinner
タップするとリストが表示され、１つだけ項目を選択できる。


Spinnerオブジェクトメソッド

|メソッド|説明|
|---|---|
|<Object> getSelectedItem| アイテムに登録されたオブジェクトを取得|
|Long getSelectedId|選択されたアイテムの番号を取得する|

xmlだけでリストを表示する方法

![](http://sunsunsoft.com/image/android/spinner_sample.png)

```xml
layout.xml

<Spinner
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/spinner"
        android:layout_below="@+id/textView"
        android:entries="@array/spnEntries"
        android:layout_alignParentStart="true"/>
```

```xml
array.xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="spnEntries">
        <item>アイテム 1</item>
        <item>アイテム 2</item>
        <item>アイテム 3</item>
    </string-array>
</resources>

```

###コードで追加
プログラム開始してから項目を追加する方法。

レイアウトにSnipperを追加する

```xml
layout.xml

<Spinner
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/spinner2"
    />
```

```java
Spinner   　spinner2 = (Spinner)findViewById(R.id.spinner2);
ArrayAdapter<String>adapter = new ArrayAdapter<String>(this,android.R.layout
        .simple_spinner_dropdown_item);
adapter.add("hoge1");
adapter.add("hoge2");
adapter.add("hoge3");
adapter.add("hoge4");
adapter.add("hoge5");
spinner2.setAdapter(adapter);
```

![](http://sunsunsoft.com/image/android/spinner_sample2.png)

###項目が選択された時のイベント処理

**その１  個別に new AdapterViewする方法**

項目が選択された時に何かしら処理を行いたい場合は、setOnItemSelectedListener メソッドでイベント処理を登録する。

```java
spinner1 = (Spinner)findViewById(R.id.spinner);

// 項目が選択された時の処理
spinner1.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener(){
    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        String item = (String)parent.getSelectedItem();
        String str = String.format("%s (%d)が選択されました", item, parent.getSelectedItemId());
        Toast.makeText(MainActivity.this, str, Toast.LENGTH_SHORT).show();
    }
    @Override
    public void onNothingSelected(AdapterView<?> arg0) {

    }
});

```

** その２ onItemSelectedメソッドを共有する**

こちらはActivityに OnItemSelectedListenerをimplementsしてonItemSelectedを使い回す方法。

```java
import android.widget.AdapterView.OnItemSelectedListener;

public class MenuActivity extends AppCompatActivity implements OnItemSelectedListener{

    // 項目に表示する文字列 
    public static final String[] menuItems = new String[]{
            "test1", "test2", "test3"};

    private Spinner spinner1;
    private Spinner spinner2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);

        // プログラムで項目を追加する
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,android.R.layout
                .simple_spinner_dropdown_item);
        for (String menu : menuItems) {
            adapter.add(menu);
        }
        
        spinner1 = (Spinner)findViewById(R.id.spinner);
        spinner1.setAdapter(adapter);
        spinner1.setOnItemSelectedListener(this);

        spinner2 = (Spinner)findViewById(R.id.spinner2);
        spinner2.setAdapter(adapter);
        spinner2.setOnItemSelectedListener(this);
    }


    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        Spinner spinner = (Spinner)parent;

        if (parent == spinner1) {
            Log.v("myLog", "spinner1");
        } else if (parent == spinner2) {
            Log.v("myLog", "spinner2");
        }
    }

    @Override
    public void onNothingSelected(AdapterView<?> arg0) {

    }
}

```