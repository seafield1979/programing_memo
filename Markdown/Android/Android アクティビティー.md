#アクティビティー Activity

アクティビティとは？
アプリの画面、を管理するもの。iOSでいうところの UIViewController クラス。

アクティビティの特徴

* ロジックとレイアウトを管理する。言い換えると画面と画面の処理を管理する
* スタックできる。言い換えると古い画面に戻れる。
* 別のアプリから呼び出せる(インテントと連携）



Android Studioでプロジェクトを作成した時点でデフォルトのアクティビティが１つ作成される。
また、このアクティビティに表示されるレイアウトも１つ作成される。


###レイアウトを表示する
アクティビティにレイアウト(xml)を表示するには setContentView(レイアウトのid) を使用する.

```java
// activity_main.xml を表示する
setContentView(R.layout.activity_main);
```

setContentViewは引数別にいくつある
~~~
void setContentView(int layoutResID)
void setContentView(View view)
void setContentView(View view, ViewGroup.LayoutParams params)
~~~



###プロジェクトにアクティビティを追加する
Android Studio からプロジェクトにアクティビティを追加するには
メニューの `File - New - Activity - [適当なActivity]` で追加できる。

AndroidManifest.xml を編集する

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.shutaro.testactivity">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        ここに追加したアクティビティの記述がある
        <activity android:name=".Main2Activity"></activity>
    </application>

</manifest>
```

###起動時に表示されるアクティビティを変更する
起動時に表示されるアクティビティをデフォルトのもの(MainActivity)から追加したもの(Main2Activity)に変更する

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.shutaro.testactivity">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        ここをMainActivity -> Main2Activity に変更
        <activity android:name=".Main2Activity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        ここを Main2Activity -> MainActivity に変更
        <activity android:name=".Main2Activity"></activity>
    </application>

</manifest>
```

###新しいアクティビティを立ち上げる
アプリ起動時に表示されるアクティビティとは別のアクティビティを表示する。

```java
import android.content.Intent;

// Main2Activity アクティビティを呼び出す
Intent i = new Intent(getApplicationContext(),Main2Activity.class);
もしくは
Intent i = new Intent(MainActivity.this, Main2Activity.class);

startActivity(i);
```