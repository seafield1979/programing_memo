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
public class MainFragment extends Fragment {
    public static final String FRAMGMENT_NAME = MainFragment.class.getName();
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment1, container, false);
        return view;
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

###Replace

###Show/Hide

すでに表示中のフラグメントを置き換えるにはreplaceメソッドを使用する

```java
  // FragmentManagerからFragmentTransactionを作成
  FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
  // 実際に使用するFragmentの作成
  Fragment newFragment = new <フラグメントクラス名>();
  // Fragmentを組み込む
  transaction.replace(R.id.<フラグメントを表示するGroupView>, newFragment);
  // backstackに追加
  transaction.addToBackStack(null);
  // 上記の変更を反映する
  transaction.commit();

```

###テクニック

```java
// フラグメントクラスの中で findViewById を使用する方法
View view = inflater.inflate(R.layout.main, container, false);
TextView textView = (TextView)view.findViewById(R.id.textView3);
```