#インテント Intent

インテントとは？
他のアクティビティーやアプリとやり取りを行う箱のようなもの。新しくアクティビティーを表示したい時、アプリを立ち上げたい時にインテントに呼び出す先の情報を設定して StartActivity() などをコールする。


###アクティビティーを表示する

```java
import android.content.Intent;

// Main2Activity アクティビティを呼び出す
Intent i = new Intent(getApplicationContext(),MainActivity.class);
もしくは
Intent i = new Intent(MainActivity.this, MainActivity.class);

startActivity(i);
```