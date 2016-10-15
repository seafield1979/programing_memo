#SurfaceView
描画用のスレッドを持った View を作るときは、SurfaceView を使う。
SurfaceViewはメインスレッド(UIスレッド)以外でも描画できるので他の処理の影響を受けにくい。

![](http://sunsunsoft.com/image/android/surfaceview1.png)

使用方法は SurfaceViewのサブクラスでスレッドを実行する。

ポイント

* SurfaceViewのサブクラスを作る
* SurfaceHolder.Callbackインターフェースをimplement。以下のメソッドを実装する
    + void surfaceCreated(SurfaceHolder holder)
    + void surfaceChanged(SurfaceHolder holder,int format,int width,int height)
    + void surfaceDestroyed(SurfaceHolder holder)
* Runnableインターフェースをimplement。runメソッドを実装する

```java
public class MySurfaceView1 extends SurfaceView implements Runnable,SurfaceHolder.Callback {
    static final long FPS = 30;
    static final long FRAME_TIME = 1000 / FPS;
    static final int BALL_R = 30;

    SurfaceHolder surfaceHolder;
    Thread thread;
    int cx = 100, cy = 100;
    int screen_width, screen_height;

    public MySurfaceView1(Context context) {
        super(context);

        surfaceHolder = getHolder();
        surfaceHolder.addCallback(this);
    }

    // Surfaceが作られた時のイベント処理
    @Override
    public void surfaceCreated(SurfaceHolder holder) {
        thread = new Thread(this);
        thread.start();
    }

    // Surfaceが変更された時の処理
    @Override
    public void surfaceChanged(
            SurfaceHolder holder,
            int format,
            int width,
            int height) {
        screen_width = width;
        screen_height = height;
    }

    // Surfaceが破棄された時の処理
    @Override
    public void surfaceDestroyed(SurfaceHolder holder) {
        thread = null;
    }

    // スレッド処理
    @Override
    public void run() {
        Canvas canvas = null;
        Paint paint = new Paint();
        Paint bgPaint = new Paint();

        // Background
        bgPaint.setStyle(Paint.Style.FILL);
        bgPaint.setColor(Color.WHITE);
        // Ball
        paint.setStyle(Paint.Style.FILL);
        paint.setColor(Color.BLUE);

        long loopCount = 0;
        long waitTime = 0;
        long startTime = System.currentTimeMillis();

        // 描画ループ
        while(thread != null){
            try{
                loopCount++;
                // 描画処理開始

                canvas = surfaceHolder.lockCanvas();
                // 背景
                canvas.drawRect( 0, 0, screen_width, screen_height, bgPaint);
                // オブジェクト
                canvas.drawCircle(cx, cy, BALL_R, paint);

                // 描画処理終了
                surfaceHolder.unlockCanvasAndPost(canvas);

                // 次のフレーム開始まで
                waitTime = (loopCount * FRAME_TIME)
                        - (System.currentTimeMillis() - startTime);

                if( waitTime > 0 ){
                    Thread.sleep(waitTime);
                }
            }
            catch(Exception e){}
        }
    }
}

```