＃基本 basic


##ログ出力

```java
// コンソールに文字列出力
System.out.println(文字列);
```

```java
// Log シリーズ
// Lod.d(String tag, String msg);

// Log.v：VERVOSE（すべてのログ情報）
// Log.d：DEBUG(デバッグ情報）
// Log.i：INFO(情報）
// Log.w：WARN(警告）
// Log.e：ERROR（致命的な問題）

Log.v("LifeCycle", "onStart");

```

##アプリを終了する

### finish
finish()
アクティビティを閉じる際の最良の終了方法。
バックボタンを押した時と同じ動きで、実行すると onPause(), onDestroy()が順番に呼ばれる。


### moveTaskToBack 
アプリを終了、ではなくクローズする。アプリは生きているのでタスクリストからまたアプリを呼び出すことができる。

moveTaskToBack(true);

