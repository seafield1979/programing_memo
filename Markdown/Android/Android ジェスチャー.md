#ジェスチャー gesture

タッチ、長押し、スワイプ等の操作を補足するにはジェスチャー

```java
public class MainActivity extends AppCompatActivity {

    private GestureDetector mGestureDetector;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // GestureDetectorインスタンス作成
        mGestureDetector = new GestureDetector(this, mOnGestureListener);
    }
    
    /**
     * ジェスチャーのリスナー
     */
    private final GestureDetector.SimpleOnGestureListener mOnGestureListener = new GestureDetector.SimpleOnGestureListener() 
    {
        // このブロック内に処理したいジェスチャーのメソッドを記述する
        @Override
        public boolean onSingleTapUp(MotionEvent e) {
            Log.d("myLog", "Single Tap " + e.getX() + " " + e.getY());
            return false;
        }
        
        ・
        ・
        ・
    }
}
```

###ジェスチャーイベントの種類
GestureDetectorで取得できるイベントの種類

|イベントメソッド|内容|
|---|---|
|onDown(MotionEvent e) |	押下時に呼ばれる
|onShowPress(MotionEvent e) |	押下時に呼ばれる(押してすぐに動かすと呼ばれない)
|onLongPress(MotionEvent e) |	長押し時に呼ばれる
|onFling(MotionEvent e1,<br> MotionEvent e2,<br>float velocityX,<br> float velocityY) |	フリック時に呼ばれる
|onScroll(MotionEvent e1,<br> MotionEvent e2,<br> float distanceX,<br>float distanceY) |	スクロール時に呼ばれる
|onSingleTapUp(MotionEvent e) |	シングルタップ時に呼ばれる（ダブルタップ時も呼ばれる）
|onSingleTapConfirmed(MotionEvent e) |	シングルタップ時に呼ばれる（ダブルタップ時は呼ばれない）
|onDoubleTap(MotionEvent e) |	ダブルタップ時に呼ばれる
|onDoubleTapEvent(MotionEvent e) |	ダブルタップイベント時に呼ばれる(押す・動かす・離す)


###MotionEvent
MotionEvent から取得できる情報

|プロパティ|内容|
|---|---|
|getX(), getY()|タップ位置等のx,y座標|
|getAction()|	タッチイベントのアクション|
|getDownTime()|	押されていた時間(ms単位)|
|getEdgeFlags()|	スクリーン端判定|
|getSize()|	タッチされている範囲、サイズ（推定）|
|getEventTime()|	タッチされていた継続時間(ms単位)|
|getPressure()|	タッチされた圧力|

###ジェスチャーイベントの順番
一連の動作で発生するイベント

**シングルタップ時**
onDown → onShowPress※ → onSingleTapUp → onSingleTapConfirmed

**長押し時**
onDown → onShowPress → onLongPress

**スクロール時（押してグイーっと動かす）**
onDown → onShowPress※ → onScroll x n

**フリック時（押してサッと動かして離す）**
onDown → onShowPress※ → onScroll x n → onFling