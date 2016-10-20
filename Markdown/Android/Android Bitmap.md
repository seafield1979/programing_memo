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

###ピクセルを自前で描画するサンプル
画像を元にBitmapとそれに対応するピクセルのバッファを用意し、バッファを更新->Bitmapに転送という形でBitmapを更新する。

```java
/**
 * ビットマップ操作のサンプルのためのView
 */
public class MyBitmapView extends ImageView
{
    private static final int RECT_W = 20;       // タッチ時に色を更新する矩形の幅
    private int bitmap_width_px;
    private int bitmap_height_px;

    // canvas関連
    private boolean isCanvasInitFinished = false;

    // bitmap関連
    private Bitmap bitmap;
    private int[] pixels;
    private Paint paintForBitmap;

    /**
     * Viewを初期化
     */
    public MyBitmapView(Context context, AttributeSet attr) {
        super(context, attr);
    }


    /**
     * invalidate()で更新される
     * @param canvas
     */
    @Override
    protected void onDraw(Canvas canvas)
    {
        // 初回のみビットマップ情報を初期化
        if( ! isCanvasInitFinished )
        {
            // onDrawの初回
            initCanvas( canvas );

            isCanvasInitFinished = true;
        }
        else
        {
            // onDrawでcanvasにbitmapを描画
            canvas.drawBitmap(
                    bitmap,
                    0, 0, // 描画座標のオフセット
                    paintForBitmap
            );
        }
    }

    /**
     * 初回のセットアップ処理
     */
    private void initCanvas(Canvas canvas)
    {
        // 空のbitmapを作成する場合
        //bitmap = Bitmap.createBitmap(bitmap_width_px, bitmap_height_px, Bitmap.Config.ARGB_8888);

        bitmap_width_px = getWidth();
        bitmap_height_px = getHeight();

        // bitmapを画像リソースから読み込む場合
        bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.hogeman);
        // 編集可能なコピーを作成
        bitmap = bitmap.copy(Bitmap.Config.ARGB_8888, true);
        // BitmapをView全体にスケール
        bitmap = Bitmap.createScaledBitmap(bitmap, bitmap_width_px, bitmap_height_px, false);

        // bitmapにロードするためのピクセル配列を作成
        pixels = new int[bitmap_width_px * bitmap_height_px];

        loadPixelsFromBitmap();

        // bitmapの描画を実行するためのPaintを準備
        paintForBitmap = new Paint();

        // 画面に反映
        invalidate();
    }

    @Override
    public boolean onTouchEvent(MotionEvent e)
    {
        if (e.getAction() != MotionEvent.ACTION_DOWN) {
            return false;
        }
        // ビットマップを更新する
        modifyBitmapOnTouch((int)e.getX(), (int)e.getY());

        // 表示状態に反映させる
        invalidate();

        return true;
    }

    /**
     * ビットマップを更新する
     */
    private void modifyBitmapOnTouch(int touch_x, int touch_y)
    {
        int sx = touch_x - RECT_W/2;
        int sy = touch_y - RECT_W/2;

        // ピクセル操作
        handlePixelsOfBitmap(sx, sy, RECT_W, RECT_W);

        // 全ピクセルを反映
        updateBitmapPixels();
    }


    /**
     * ピクセルレベルでビットマップを操作する
     */
    private void handlePixelsOfBitmap(int sx, int sy, int width, int height)
    {
        // 該当ピクセルの周辺を操作
        int targetIndex;
        Point targetPoint;
        for( int y = sy; y < sy + width; y ++) {
            for( int x = sx; x < sx + height; x ++) {
                targetPoint = new Point( x, y );
                targetIndex = point2bitmapIndex( targetPoint );

                // このピクセルを操作
                modifyOnePixelByIndex(targetIndex);
            }
        }
    }

    /**
     * ビットマップからピクセルをロード
     */
    private void loadPixelsFromBitmap() {
        // bitmap のピクセルを pixels に読み込む
        bitmap.getPixels(pixels, 0, bitmap_width_px, 0, 0, bitmap_width_px, bitmap_height_px);
    }

    /**
     * Bitmap内の全ピクセルを更新
     */
    private void updateBitmapPixels() {
        bitmap.setPixels(pixels, 0, bitmap_width_px, 0, 0, bitmap_width_px, bitmap_height_px );
    }


    /**
     * ビットマップ上の座標から，ピクセル配列のインデックスに変換
     */
    private int point2bitmapIndex(Point p)
    {
        return p.y * bitmap_width_px + p.x;
    }


    /**
     * 1ピクセルを操作
     */
    private void modifyOnePixelByIndex(int targetIndex)
    {
        // 配列の範囲外は防止
        if( pixels.length <= targetIndex ) return;

        // 範囲内の正常なインデックスの場合
        int targetPixel = pixels[ targetIndex ];

        // 対象ピクセルを更新（色を反転)
        pixels[ targetIndex ] = Color.argb(
            128,
            255-Color.red(targetPixel),
            255-Color.green(targetPixel),
            255-Color.blue(targetPixel)
        );
    }
}
```

```java
// レイアウトには以下のように配置する
<パッケージ名.MyBitmapView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:srcCompat="@drawable/hogeman"
        android:id="@+id/imageView"/>
```