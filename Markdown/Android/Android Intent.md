#インテント Intent

インテントとは？
他のアクティビティーやアプリとやり取りを行う箱のようなもの。新しくアクティビティーを表示したい時、アプリを立ち上げたい時にインテントに呼び出す先の情報を設定して StartActivity() などをコールする。


###アクティビティーを起動する

```java
import android.content.Intent;

// Main2Activity アクティビティを呼び出す
Intent i = new Intent(getApplicationContext(),MainActivity.class);
もしくは
Intent i = new Intent(MainActivity.this, MainActivity.class);

startActivity(i);
```

###呼び出し先にパラメータを渡す
put~系のメソッドで呼び出し先のアクティビティーにパラメータを渡す。


* 呼び出し元で startActivityForResult でアクティビティを呼び出す
* 呼び出し元で戻り値を受け取るメソッド onActivityResult を実装
* 呼び出し先でアクティビティを終了する前にsetResult(RESULT_OK, <Intentオブジェクト>);で戻り値を渡す

```java
呼び出し元のActivity

public class MainActivity extends AppCompatActivity implements OnClickListener {
  ・
  ・
  ・
  private static final int REQUEST_CODE = 1;
  
  void test1() // 適当なメソッド
  {
    // Main2Activity アクティビティを呼び出す
    Intent intent = new Intent(getApplicationContext(),Main2Activity.class);
    startActivityForResult(intent, REQUEST_CODE);
  }
  
  // 戻り値を受け取るメソッド
  @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_CODE) {
            if (resultCode == RESULT_OK) {
                String value = data.getStringExtra("key_name");
                TextView result = (TextView) findViewById(R.id.textViewResult);
                result.setText(value);
            }
        }
    }
}


呼び出し先のActivity
public class Main2Activity extends AppCompatActivity {
  ・
  ・
  ・
  Intent data = new Intent();
  data.putExtra("key_name", "もどったぜ");
  setResult(RESULT_OK, data);
  finish();
}
```

###指定のアクションを持つアクティビティーを起動する

メール送信用のアクティビティを立ち上げる

```java
// 指定のアクションを持つアクティビティを起動する(複数ある場合はユーザーが選択できる)
Intent intent = new Intent();
intent.setAction(Intent.ACTION_SENDTO);

//宛先をセット
intent.setData(Uri.parse("mailto:hogehogew@gmail.com"));

//標題をセット
intent.putExtra(Intent.EXTRA_SUBJECT, "XXX");

//本文をセット
intent.putExtra(Intent.EXTRA_TEXT, "本文");
startActivity(intent);
```

###指定のアクションを持つアプリの一覧を取得
指定のアクション(ACTION_SENDTOやACTION_VIEW等)の一覧を取得する

![](http://sunsunsoft.com/image/android/app_info_list.png)
↑ACTION_SEARCH アクションのアプリ一覧を取得

```java
PackageManager pm = getPackageManager();
Intent intent = new Intent();
intent.setAction(Intent.ACTION_SEARCH);

List<ResolveInfo> activities = pm.queryIntentActivities(intent, PackageManager
        .MATCH_ALL);

for (ResolveInfo info : activities) {
    Log.v("myLog", (String)info.loadLabel(pm));
}
```