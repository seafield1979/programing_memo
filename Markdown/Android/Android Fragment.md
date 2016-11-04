#フラグメント fragment

[Android はじめてのFragment](http://qiita.com/Reyurnible/items/dffd70144da213e1208b)

[Androidアプリ開発メモ Fragmentを使う](http://humitomotti.hateblo.jp/entry/2015/09/21/035221)


Fragmentとは、簡単にいうと、コンテンツとライフサイクルを持ったビューです。

* Android 3系で追加された(以前はActivityオンリー)

###フラグメントの特徴

* 親子関係が持てる(フラグメントの中にフラグメンを生成できる）
* 複数の画面で使いまわせる(Activityではできない)
* ライフサイクル系のメソッド呼ばれる(onCreate,onDestroy,onResume,onStart,onPause,onStop)
* ロジックを記述できる(配下のWidgetを制御したり)


###ライフサイクル
![](http://sunsunsoft.com/image/android/fragment_lifecycle.png)


操作と発生するイベント

|操作|イベント|説明|
|---|---|
|アプリ起動|onAttach<br>onCreate<br>onCreateView<br>onViewCreated<br>onActivityCreated<br>onStart<br>onResume|FragmentTransactionにadd,commitしたとき|
|アプリ終了|onPause<br>onStop<br>onDestroyView<br>onDestroy<br>onDetach|戻るボタンでアプリを終了したとき|
|ホームボタン|onPause<br>onStop|ホームボタンでホームに戻ったとき|
|再表示|onStart<br>onResume|タスク一覧から選択して再表示したとき|

Activityのライフサイクルとの対応

|Activityの状態|Fragmentイベント|
|---|---|
|Create|onAttach<br>onCreate<br>onCreateView<br>onViewCreated<br>onActivityCreated|
|Started|onStart|
|Resumed|onResume|
|Paused|onPause|
|Stopped|onStop|
|Destroyed|onDestroyView<br>onDestroy<br>onDetach|



###使い方

フラグメントを作成する (メニューの`File - New - Fragment`) から適当なFragmentを選択

メインアクティビティにフラグメントを挿入する

サンプルで用意するファイルは以下
~~~
app
 + java
   + MainActivity.java   メインのアクティビティクラス
   + Fragment1.java       フラグメイントクラス
res
 + layout
   + activity_main.xml   メインアクティビティのlayout
   + fragment1.xml       フラグメントのlayout
~~~


####1 フラグメント用のレイアウト(xlm)を作成
  メニューから `File - New - Fragment` で適当なフラグメントを追加
  ![](http://sunsunsoft.com/image/android/new_fragment.png)

####2 フラグメント用のクラス(Fragmentを継承)を作成
  クラス作成から Fragment1.java を作成
  

```java
package com.example.shutaro.testfragment;

import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

/**
 * Created by shutaro on 2016/09/28.
 */
public class MainFragment extends Fragment implements OnClickListener{
    public static final String FRAMGMENT_NAME = MainFragment.class.getName();
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment1, container, false);
        return view;
    }
    
    // Viewが生成し終わった時に呼ばれるメソッド
    @Override
    public void onViewCreated(View view, Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        
        // メンバ変数のヒモ付けやイベントリスナーの登録をしたりする
        Button button = (Button)findViewById(R.id.button);
        button.setOnClickListener(this);
    }
}
```

####3 アクティビティのレイアウト(xlm)にフラグメントを表示するための領域(コンテナ）を追加する

activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.shutaro.testfragment.MainActivity">

    <!-- フラグメントコンテナ -->
    <FrameLayout
        android:id="@+id/fragment_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_alignParentEnd="true" />

</RelativeLayout>

```

####4 フラグメイントを生成、表示するコードを記述する

フラグメントにデータを渡したい場合は Bundle クラスを使用する
フラグメントの生成、コンテナにaddする

MainActivity.java

```java
import android.support.v4.app.FragmentTransaction;  // 追加

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        // 追加↓↓↓↓↓↓↓↓↓ 
        if (savedInstanceState == null) {
            // データを渡す為のBundleを生成し、渡すデータを内包させる
            Bundle bundle = new Bundle();
            bundle.putString("URL", "http://hogehoge.com");

            // Fragmentを生成し、setArgumentsで先ほどのbundleをセットする
            MainFragment fragment = new MainFragment();
            fragment.setArguments(bundle);

            FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
            // コンテナにMainFragmentを格納
            transaction.add(R.id.fragment_container, fragment, MainFragment.FRAMGMENT_NAME);
            // 画面に表示
            transaction.commit();
        }
        // ↑↑↑↑↑↑↑↑↑↑
    }
}
```

####5 フラグメイントクラスの処理を記述する
生成元から渡されたデータを受け取る。

Fragment1.java

```java
public class MainFragment extends Fragment {
    public static final String FRAMGMENT_NAME = MainFragment.class.getName();
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment1, container, false);
        
        // 渡された引数を取得する
        Bundle bundle = getArguments();
        String url = bundle.getString("URL");

        // 渡された文字列(url)をTextViewに表示する
        TextView textView = (TextView)view.findViewById(R.id.textView3);
        textView.setText(url);

        return view;
    }
}
```

##フラグメントを切り替える
表示するフラグメントを切り替える方法はいくつかある。

###Add/Remove
FragmentTransaction.add() でViewGroupにFragmentを追加できる。一度に複数のFragmentを追加できる。複数追加した場合は最後に追加したものが表示される。

```java
// FragmentManagerからFragmentTransactionを作成
FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();

// コンテナにMainFragmentを格納
this.fragment1 = new Fragment1();
transaction.add(R.id.fragment_container, fragment1, Fragment1.FRAMGMENT_NAME);

this.fragment2 = new Fragment2();
transaction.add(R.id.fragment_container, fragment2, Fragment2.FRAMGMENT_NAME);

this.fragment3 = new Fragment3();
transaction.add(R.id.fragment_container, fragment3, Fragment3.FRAMGMENT_NAME);

// 上記の変更を反映する
transaction.commit();
```

###Replace
すでに表示中のフラグメントを置き換えるにはreplaceメソッドを使用する

```java
  // FragmentManagerからFragmentTransactionを作成
  FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
  // 実際に使用するFragmentの作成
  Fragment newFragment = new <フラグメントクラス名>();
  // Fragmentを組み込む
  transaction.replace(R.id.<フラグメントを表示するViewGroup>, newFragment);
  // backstackに追加
  transaction.addToBackStack(null);
  // 上記の変更を反映する
  transaction.commit();
```

###Show/Hide
指定のFragmentの表示を切り替える

```java
// show/hideするFragmentをaddしておく
FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
this.fragment1 = new Fragment1();
transaction.add(R.id.fragment_container, fragment1, Fragment1.FRAMGMENT_NAME);
this.fragment2 = new Fragment2();
transaction.add(R.id.fragment_container, fragment2, Fragment2.FRAMGMENT_NAME);
transaction.commit();

// fragment1を表示,fragment2を隠す
FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
transaction.show(fragment1);
transaction.hide(fragment2);
transaction.commit();
```

###テクニック

フラグメント内のViewを参照する方法
inflater.inflate が返すViewオブジェクトを使用してfindViewByIdを呼び出す。

```java
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    View view = inflater.inflate(R.layout.fragment21, container, false);

    textView = (TextView)view.findViewById(R.id.textView);
    textView.append("hogehogehoge\n");

    return view;
}
```