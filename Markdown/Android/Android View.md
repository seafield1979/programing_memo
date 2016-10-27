#View
ただのView。レイアウトにはViewは配置できないのでコードで配置

```java
-- Activity --
// mContainerはLinearLayout
View view = new View(this);
view.setBackgroundColor(Color.rgb(255,255,0));
view.setLayoutParams(new LinearLayout.LayoutParams(100,100));
mContainer.addView(view);

-- Fragment --
View view = new View(getActivity());
view.setBackgroundColor(Color.rgb(255,255,0));
view.setLayoutParams(new LinearLayout.LayoutParams(100,100));
mContainer.addView(view);
```

###Viewのイベント

|イベント|説明|
|---|---|
|onMeasure|自分自身の幅高さを確定させるもの|
|onLayout|子Viewの位置を決めるもの|



###Viewのサブクラスを作成
Viewクラスを継承したクラスを作成する。
画面表示処理はonDrawをオーバーライドする。

```java
public class MyView extends View {
    private Paint paint = new Paint();

    public MyView(Context context) {
        super(context);
    }
    public MyView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    public void onDraw(Canvas canvas) {
        // 背景塗りつぶし
        canvas.drawColor(Color.argb(0,0,0,0));
        
        // 円を描画
        // 線の種類
        paint.setStyle(Paint.Style.STROKE);
        // 線の太さ
        paint.setStrokeWidth(10);
        // 色
        paint.setColor(Color.argb(128,255,0,0));

        canvas.drawCircle(getWidth()/2, getHeight()/2, getWidth()/2 - 5, paint);
    }
}

```

###動的にサイズを変更する
Viewのサイズを変更するには View.setLayoutParams() だけではだめで、このメソッドの後に呼ばれるViewのonMeasure()でViewのサイズを設定する必要がある。

```java
View.setLayoutParams(new LinearLayout.LayoutParams(100, 100));
 
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    int widthMode = MeasureSpec.getMode(widthMeasureSpec);
    int heightMode = MeasureSpec.getMode(heightMeasureSpec);
    int widthSize = MeasureSpec.getSize(widthMeasureSpec);
    int heightSize = MeasureSpec.getSize(heightMeasureSpec);
    
    int width = widthMode | widthSize;
    int height = heightMode | heightSize;
    setMeasuredDimension(width, height);
}
```