#EditText


|戻り値|メソッド|説明|
|---|---|---|
|Editable|String getText()|文字列を取得する|
|Editable|setText(String)|文字列を設定する|


###ヒント(プレースホルダー)を表示する
android:hint に テキスト未入力時にのみ表示するテキストを設定できる。
![](http://sunsunsoft.com/image/android/edittext_hint.png)

```xml
layout.xml
<EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="You name"
    android:id="@+id/editText"/>
```

###テキストをセット、ゲットする

```java
mEditText = (EditText)findViewById(R.id.editText);
mEditText.setText("hoge");
String str = mEditText.getText().toString();
```