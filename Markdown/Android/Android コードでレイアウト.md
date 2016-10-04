#コードでレイアウト layout by code

xmlでのレイアウトは [Android レイアウト](quiver:///notes/FA86DDEC-1B76-4025-AA91-63A8AC5133BE) を参照

###サイズを設定する
LinearLayout.LayoutParams(width,height)でサイズの計算方法のパラメータを指定する

```java
Button button = new Button();
button.setLayoutParams(new LinearLayout.LayoutParams(
            LinerLayout.LayoutParams.MATCH_PARENT,
            LinerLayout.LayoutParams.WRAP_CONTENT));
```

LinearLayout.LayoutParams で設定できるパラメータ

|パラメータ|説明|
|---|---|
|LinerLayout.LayoutParams.MATCH_PARENT| 親のサイズと同じ(width/height)|
|LinerLayout.LayoutParams.WRAP_CONTENT| コンテンツのサイズ(width/height)|

ピクセルで設定する場合は以下のメソッドを使用する。

|メソッド|説明|
|---|---|
|setWidth(width)|幅設定|
|setHeight(height)|高さ設定|

```java
button.setWidth(100);
button.setHeight(50);
```

###RelativeLayout
RelativeLayoutで配置したView(button3)の下にボタンを追加

```java
Button newButton = new Button(this);
newButton.setText("新しいボタン");

RelativeLayout.LayoutParams layout = new RelativeLayout.LayoutParams(
                RelativeLayout.LayoutParams.WRAP_CONTENT,
                RelativeLayout.LayoutParams.WRAP_CONTENT);
layout.addRule(RelativeLayout.BELOW, R.id.button3);
layout.addRule(RelativeLayout.CENTER_HORIZONTAL);
button5.setLayoutParams(layout);
        
// ViewGroupに追加
RelativeLayout relativeLayout = (RelativeLayout)findViewById(R.id.relativeLayout);
relativeLayout.addView(button5);
```

RelativeLayout.LayoutParams.addRuleで使用出来るパラメータ

|パラメータ|説明|
|---|---|
|ABOVE | 指定Viewの上に配置
|BELOW | 指定Viewの下に配置
|END_OF<br>RIGHT_OF | 指定Viewの右側に配置
|START_OF<br>LEFT_OF | 指定Viewの左側に配置
|ALIGN_BASELINE | 別のViewのベースラインを合わせる
|ALIGN_TOP | 別のViewと上端をわせる
|ALIGN_BOTTOM | 別のViewと下端を合わせる
|ALIGN_RIGHT<br>ALIGN_END | 別のViewと右端を合わせる
|ALIGN_LEFT<br>ALIGN_START | 別のViewと
|ALIGN_PARENT_TOP | 親Viewと上端を合わせる
|ALIGN_PARENT_BOTTOM | 親Viewと下端を合わせる
|ALIGN_PARENT_RIGHT<br>ALIGN_PARENT_END | 親Viewと右端を合わせる
|ALIGN_PARENT_LEFT<br>ALIGN_PARENT_START | 親Viewと左端を合わせる
|CENTER_HORIZONTAL | 親の領域の中心(水平)
|CENTER_IN_PARENT | 親の領域の中心
|CENTER_VERTICAL | 親の領域の中心(垂直)

### 文字の位置を設定(Gravity)
文字をコンテンツ領域のどちらに寄せて表示させるか

setGravityメソッドを使用する。

```java
text01.setGravity(Gravity.CENTER);
text02.setGravity(Gravity.RIGHT);
text03.setGravity(Gravity.TOP|Gravity.CENTER);
text04.setGravity(Gravity.LEFT|Gravity.BOTTOM);
```

setGravityで設定できるパラメータ。

|パラメータ|説明|
|---|---|
|CENTER|水平、垂直で中心位置|
|CENTER_VERTICAL|垂直で中心位置|
|CENTER_HORIZONTAL|水平で中心位置|
|TOP|上端|
|BOTTOM|下端|
|LEFT|左端|
|RIGHT|右端|