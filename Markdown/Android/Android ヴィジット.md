#ヴィジット Widget
ホーム画面に常に表示される小さなアプリ。一定時間ごとに更新処理が呼ばれて表示を更新することができる。

特徴

* ホーム画面に固定領域を持ち、情報を表示する。
* タップされた時に処理を行うことができる


##リンク
[Hello Widget!　(1)　最小構成のWidget](http://mrstar-memo.hatenablog.com/entry/2012/10/07/141852)
[Hello Widget!　(2)　AppWidgetProviderについて](http://mrstar-memo.hatenablog.com/entry/2012/10/25/012602)

タイマーで一定時間間隔でヴィジットを更新する方法
[API Level 19以降でAlarmManagerが辛かった話](http://santea.hateblo.jp/entry/2015/12/23/235158)


![](http://image.itmedia.co.jp/ait/articles/1204/20/r5zu01.jpg)


###Hello Widget
一番簡単なWidgetアプリを作る。といってもほとんどAndroid Studioが出力するソースを使うだけ。

1.プロジェクトを作成する
2.File - New - Widget - App Widget で Widgetを追加
3.アプリをビルド実行(Cmd + R)

これでWidgetが端末にインストールされる。
Widgetを表示するには端末のホーム画面を長押し - Widget一覧からインストールされた Widgetを選択してホームに配置する。


###AppWidgetProvierInfo
AppWidgetProvierInfoはヴィジットの情報が入ったクラス。
値の参照方法

```xml
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        // 自分のWidgetの情報を取得
        ComponentName name = new ComponentName(this, NewAppWidget.class);
        AppWidgetManager appWidgetManager = AppWidgetManager.getInstance(this);
        int[] ids = appWidgetManager.getAppWidgetIds(name);
        for (int id : ids) {
            AppWidgetProviderInfo info = appWidgetManager.getAppWidgetInfo(id);
            Log.d("myLog", info.loadLabel(this.getPackageManager()));
        }
        
        // インストールされている全てのWidgetの情報を取得
        // 現在インストールされているWidgetのリストを取得
        AppWidgetManager manager = AppWidgetManager.getInstance(this);
        List<AppWidgetProviderInfo> widgetList = manager.getInstalledProviders();

        for(AppWidgetProviderInfo info : widgetList) {
          Log.d("label", info.loadLabel(this.getPackageManager()));
        }
    }
}
```

AppWidgetProviderInfo

|AppWidgetProviderInfo|appwidget-provider(xml)|説明|
|---|---|---|
|int autoAdvanceViewId | android:autoAdvanceViewId | AppWidget subview の view ID を指定します。AppWidget subview は widget のホストによって自動切り替えされるものです。
|ComponentName configure | android:configure ||
|int icon | android:icon |(AndroidManifest.xml の <receiver>)
|int initialLayout | android:initialLayout|(AppWidget meta-data file)
|String label | android:label |(AndroidManifest.xml の <receiver>)
|int minHeight | android:minHeight |(AppWidget meta-data file)
|int minWidth | android:minWidth |指定するサイズは次の式が推奨されています。(number of cells * 74) - 2 [dp]
|int previewImage | android:previewImage|、ユーザーが追加する AppWidget を選択するときに、実際に追加したときの見た目を確認できるようにするためのものです。指定されていない場合は通常のランチャーアイコンが表示されます。
|ComponentName provider | android:name|(AndroidManifest.xml の <receiver>)
|int resizeMode | android:resizeMode|Widgetがリサイズできるかどうかのルールを指定します。指定できる値は "horizontal", "vertical", "none", "horizontal","vertical" です。
|int updatePeriodMillis | android:updatePeriodMillis |どのくらいの頻度で AppWidgetProvider から App Widget framework に updateを依頼するかを宣言します。<br>注意：３０分に配送されるアップデートリクエストは１回まで

```xml
~_app_widget_info.xml
<appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
                    android:initialKeyguardLayout="@layout/new_app_widget"
                    android:initialLayout="@layout/new_app_widget"
                    android:minHeight="40dp"
                    android:minWidth="250dp"
                    android:previewImage="@drawable/example_appwidget_preview"
                    android:resizeMode="horizontal|vertical"
                    android:updatePeriodMillis="86400000"
                    android:widgetCategory="home_screen">
</appwidget-provider>
```

###AppWidgetProviderのメソッド

**onUpdate**
~~~java
void onUpdate(Context context, AppWidgetManager appWidgetManager, int[] appWidgetIds)~~~
このメソッドは，アップウィジェットを更新するために呼ばれる。
間隔は，AppWidgetProviderInfoの中のupdatePeriodMillisで指定した間隔で呼ばれる。
また，アップウィジェットをウィジェットを初期化した時にも呼ばれる。
そのためもし必要であれば，ビューを扱うイベントハンドラーを定義したりするべき。
ウィジェットの設定Activityを使う場合は，
アップウィジェットを初期化した時このメソッドが呼ばれないことに注意しないといけない。

**onDeleted**
~~~java
void onDeleted(Context context, int[] appWidgetIds)
~~~
アップウィジェットがアップウィジェットホストから消されるごとに呼ばれる。


**onEnabled**
~~~java
void onEnabled(Context context)
~~~
初めてアップウィジェットのインスタンスが生成されたときに呼ばれる。
二つ同じ種類のアップウィジェットを置いたとしても一回しか呼ばれない。
データベースを初期化生成したリだとか，
全てのアップウィジェットインスタンスに対して一回やればいいという処理はここ。

**onDisabled**
~~~java
void onDisabled(Context context)
~~~
アップウィジェットの最後のインスタンスがアップウィジェットホストから消される時呼ばれる。
一時的なデータベースの削除など，onEnabledメソッドでやったことに対しての後処理はここでやるべきらしい。

**onAppWidgetOptionsChanged**
~~~java
void onAppWidgetOptionsChanged(Context context, AppWidgetManager
appWidgetManager, int appWidgetId, Bundle newOptions)
~~~
ウィジェットが初めて置かれた時と，サイズが変わったときに呼ばれる。
ウィジェットのサイズを取得できる。
(APIレベル16から)


###頻繁に更新したいWidget
android:updatePeriodMillis は最短の更新時間が30分。
時計など頻繁に更新が必要なウィジェットを作成したい場合、これは困るので、この周期を利用せず、AlarmManagerを用いて更新するための処理を起動させるようにする方法があります(今回は試しにこちらで書きました)。