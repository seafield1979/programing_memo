#TextView

TextViewはテキストを表示するためのViewクラス。

~~~
Object
  + View
    + TextView
~~~
主なメソッド

|メソッド|説明|
|---|---|
|setText(String)|表示するテキストを設定|
|getText()|表示中のテキストを参照|
|setTextSize(float size)|テキストのサイズを設定|
|setTextColor(int color)|テキストの色を設定|
|setBackgroundColor(int color)|背景色を設定|
|setShadowLayer(radius, x, y, color)|テキストの影を設定|
|setTypeface(Typeface tf, int style)|テキストのタイプ(太文字、斜体等)を設定|

### アイコンを表示する
![](http://sunsunsoft.com/image/android/textview_show_icon.png)

```swift
// 画像を表示 (robot.png)
Drawable image = ResourcesCompat.getDrawable(getResources(), R.drawable.robot, null);
// 画像サイズを指定(これをしないと表示されない)
image.setBounds(0, 0, image.getIntrinsicWidth(), image.getIntrinsicHeight());

textViews.setWidth(500);
textViews.setHeight(200);
textViews.setGravity(Gravity.CENTER_VERTICAL);
textViews.setBackgroundColor(Color.rgb(200,200,200));
textViews.setCompoundDrawables(image, null, null, null);
```

###テキスト表示位置を設定する
setGravityでテキストの表示位置を枠内の上下左右に合わせることができる。
![](http://sunsunsoft.com/image/android/textview_gravity.png)

```java
//縦横中心に表示
textView.setGravity(Gravity.CENTER);

//左側に表示
textView.setGravity(Gravity.LEFT | Gravity.TOP);
```

setGravityで使用できるプロパティ

|プロパティ|説明|
|---|---|
|BOTTOM|	要素を下方向に寄せる|
|CENTER|	要素をViewの真ん中に寄せる|
|CENTER_HORIZONTAL|	要素をViewの横幅の真ん中に寄せる|
|CENTER_VERTICAL|	要素をViewの縦幅の真ん中に寄せる|
|LEFT|	要素を左方向に寄せる|
|RIGHT|	要素を右方向に寄せる|
|TOP|	要素を上方向に寄せる|