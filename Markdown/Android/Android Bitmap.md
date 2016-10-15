#Bitmap

###Bitmapを読み込む
リソースからBitmapを生成する

```java
Bitmap BitmapFactory.decodeResource(Resources res, int id);

例
Bitmap bmp = BitmapFactory.decodeResource(getResources(), R.drawable.robot2);
```

###Bitmapから拡大縮小

```java
Bitmap Bitmap.createScaledBitmap(bmp, width, height, filter);

例
Bitmap newBmp = Bitmap.createScaledBitmap(bmp, 200, 90, false);
```

###ImageViewに設定

```java
ImageView iv = (ImageView)findViewById(R.id.imageView);
Bitmap bmp = BitmapFactory.decodeResource(getResources(), R.drawable.image1);
iv.setImageBitmap(bmp);
```

###Canvasに描画
Viewクラスの継承先のクラスでonDrawをオーバーライドしてCanvasにBitmapを描画する。

`canvas.drawBitmap(Bitmapオブジェクト, x, y, Paintオブジェクト);`

```java
public class SampleView extends View{
    private Paint paint = new Paint();
    private Bitmap mBmp;

    public SampleView(Context context, AttributeSet attrs) {
        super(context, attrs);
        mBmp = BitmapFactory.decodeResource(getResources(), R.drawable.image1);
    }

    public void setDraw(){
      //再描画
      invalidate();
    }

    @Override
    public void onDraw(Canvas canvas) {
        // 背景色を設定
        canvas.drawColor(Color.WHITE);
        
        canvas.drawBitmap(mBmp, 0, 0, paint);

        canvas.scale(1.5f, 1.5f);
        canvas.drawBitmap(mBmp, 50, 50, paint);

        canvas.scale(1.5f, 1.5f);
        canvas.drawBitmap(mBmp, 100, 100, paint);
    }
}
```