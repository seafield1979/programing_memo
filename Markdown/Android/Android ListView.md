#ListView

テキストの項目を縦に表示するListViewを作成してみる。
![](http://sunsunsoft.com/image/android/listview_sample.png)

```xml
main_layout.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.shutaro.testlistview.MainActivity">

    <ListView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/listView"
        android:layout_weight="1"/>
</LinearLayout>
```

```java
main_activity.java

public class MainActivity extends Activity  implements OnItemClickListener{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ListView list1 = (ListView)findViewById(R.id.listView);
        list1.setOnItemClickListener(this);

        // Adapterの作成
        ListAdapter adapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, DAYS);
        // Adapterの設定
        list1.setAdapter(adapter);
    }
    
    // 項目がクリックされた時の処理
    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        ListView lv = (ListView)parent;
        Log.v("myLog", (String)lv.getItemAtPosition(position));
    }

    // リストに表示する文字列
    private static final String[] DAYS = new String[]{"Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"};

}
```

##GridView
縦横に項目が表示できるView

![](http://sunsunsoft.com/image/android/gridview_sample.png)

```xml
activity_layout.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.example.shutaro.testlistview.GridViewActivity">

    <GridView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:numColumns="4"
        android:verticalSpacing="10dp"
        android:horizontalSpacing="10dp"
        android:id="@+id/gridView"/>

</LinearLayout>

```

```java
grid_activity.java

import android.widget.AdapterView.OnItemClickListener;

public class GridViewActivity extends Activity implements OnItemClickListener {

    public static final String[] WEEK = new String[]{"月","火","水","木","金","土","日"};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_grid_view);

        GridView grid1 = (GridView)findViewById(R.id.gridView);
        grid1.setOnItemClickListener(this);

        // Adapter作成
        ListAdapter adapter = new ArrayAdapter(this, android.R.layout.simple_list_item_1, WEEK);
        // Adapter設定
        grid1.setAdapter(adapter);
    }

    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        GridView gv = (GridView)parent;
        Log.v("myLog", (String)gv.getItemAtPosition(position));
    }
}
```

###カスタムされた項目を返すListView

TestListViewプロジェクトのCustomListActivity を参照のこと。