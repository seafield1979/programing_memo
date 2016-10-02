#デバッグ debug


##ログ出力
デバッグコンソールに文字列出力

```java
System.out.println("hoge");
```

##ブレークポイント

###追加
ブレークポイントを設定したい行のガーター部分をクリックすると赤い●がつく。これでデバッグ実行時にこの行が実行される前に処理が停止してデバッグモードになる。

![](http://sunsunsoft.com/image/android/breakpoint1.png)
↑ブレークポイントを設定した



###ブレークポイントで停止
ブレークポイントで処理を停止させたい場合はデバッグ実行(Ctrl + D)
![](http://sunsunsoft.com/image/android/debug_button.png)
↑このボタン

でアプリを起動しないといけない。
ブレークポイントまで処理を進める、とブレークする。ブレーク中は

再開 Cmd + Opt + R
ステップイン F7
ステップオーバー F8
ステップアウト Shift + F9 

等で処理を進める。

###変数の値を変更する
ブレークポイントで停止時に Variables に表示される変数を右クリック `Set Value` を選択し、変更したい値を入力してから決定(Return)。

![](http://sunsunsoft.com/image/android/set_value1.png)
![](http://sunsunsoft.com/image/android/set_value2.png)