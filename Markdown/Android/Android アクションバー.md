#アクションバー ActionBar

###ActionBarのタイトルを変更する
Activity.getSupportActionBar() を使用する。getActionBarだとnullが返ってきて使えなかった。

```java
import android.support.v7.app.AppCompatActivity;

ActionBar ab = getSupportActionBar();
ab.setTitle("たいとるだ");
ab.setSubtitle("さぶたいとるだ");
```

###アイコンを変更する

```java
getSupportActionBar().setDisplayShowHomeEnabled(true);
getSupportActionBar().setIcon(R.mipmap.ic_launcher);
```

###戻るを追加
アクションバーの左側に←ボタンを追加して、前の画面（アクティビティ）に戻れるようにする。
![](http://sunsunsoft.com/image/android/actionbar_return.png)

```java
public class MainActivity extends AppCompatActivity implements OnClickListener {
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
          // アクションバーに戻るボタンを追加
          ActionBar ab = getSupportActionBar();
          ab.setHomeButtonEnabled(true);
          ab.setDisplayHomeAsUpEnabled(true);
    }
    
    // メニューの項目が選択されると呼ばれる
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // item.getItemId() は選択されたメニューのID
        int itemId = item.getItemId();
        
        // 戻るボタンのIDならアクティビティを終了
        if(itemId == android.R.id.home) {
          finish();
        }
    }
}
```

###メニューを追加
アクションバーの右端にメニューアイコンを表示、ここからメニューの各項目を表示できるようにする。

![](http://sunsunsoft.com/image/android/actionbar_menu1.png)
![](http://sunsunsoft.com/image/android/actionbar_menu2.png)

手順としては

* Activityクラスにメニューボタン生成の onCreateOptionsMenu メソッドを実装
    + メニュー用のlayoutを getMenuInflater() で生成してreturnする
* Activityクラスにメニュー項目が選択時に呼ばれる onOptionsItemSelected メソッドを実装

```xml
menu_layuot.xml

<menu xmlns:android="http://schemas.android.com/apk/res/android" >
    <item
        android:id="@+id/action_settings"
        android:orderInCategory="100"
        android:showAsAction="always"
        android:title="Settings"/>
    <item
        android:id="@+id/action_hp"
        android:orderInCategory="100"
        android:showAsAction="always"
        android:title="Homepage"/>
    <item
        android:id="@+id/action_s3"
        android:orderInCategory="100"
        android:showAsAction="never"
        android:title="Sunsunsoft"/>
</menu>
```

```java
public class MainActivity extends AppCompatActivity implements OnClickListener {

    // メニューボタンを生成する
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // main.xml から生成
        getMenuInflater().inflate(R.menu.main, menu);
        return super.onCreateOptionsMenu(menu);
    }

    // メニューの項目が選択されると呼ばれる
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int itemId = item.getItemId();
        String name = (String)item.getTitle();
        
        // 押された項目によって処理を行う
        switch (itemId) {
            case R.id.action_settings:
                break;
            case R.id.action_hp:
                break;
            case R.id.action_s3:
                break;
        }
        return super.onOptionsItemSelected(item);
    }
}
```

###preferences.xlm(設定画面)の項目

|クラス|説明|
|---|---|
|CheckBoxPreference	| オン/オフの設定項目
|SwitchPreference	| オン/オフの設定項目<br>API14から
|ListPreference |	リストの中から1つ選ぶ設定項目
|MultiSelectListPreference |	リストの中から複数を選ぶ設定項目
|EditTextPreference |	入力からの設定