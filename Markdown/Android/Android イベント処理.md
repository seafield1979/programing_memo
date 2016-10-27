#イベント処理 Event

タッチやスワイプ等のイベントを補足して自前の処理を行う方法。

[Android のタッチイベントを理解する(その1)](http://blog.lciel.jp/blog/2013/12/03/android-touch-event/)

イベントはや親Viewから子Viewに伝達していく。
子への伝達を行いたくない場合は親ViewのonInterceptTouchEvent()でtrueを返す

##イベントの種類

|イベント|リスナー|設定メソッド|イベントメソッド|
|---|---|---|
|クリック|OnClickListener|setOnClickListener<br>(OnTouchListener l))|void onClick(View v)|
|タッチ|OnTouchListener|setOnTouchListener<br>(OnTouchListener l)|boolean onTouch<br>(View v, MotionEvent e)|
|ドラッグ|OnDragListener|setOnDragListener<br>(OnDragListener l)|boolean onDrag<br>(View v, DragEvent event)|
|長押し|OnLongClickListener|setOnLongClickListener<br>(OnLongClickListener l)|boolean onLongClick(View v)|
  
##基本
Widgetのイベント登録方法は以下の２つのパターンがある。

###各Widgetに無名リスナーを登録する
  つながりが解りやすいが数が増えるとリスナーの数が増えてコード量が増える

**クリックイベントを無名リスナーで登録するサンプル**

```java
public class Test1Activity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
      Button button1 = (Button)findViewById(R.id.Button1);
      
      // 無名リスナを登録
      button1.setOnClickListener(new OnClickListener() {
        @Override
        public void onClick(View v) {
            Log.v("mylog", "onClick");
        }
      });
    }
}
```

  
###アクティビティのリスナーを使用する
  つながりが少々解りにくいが、リスナーの処理が１箇所に集約できるのでわかりやすい
  
**クリックイベントをアクティビティに持つサンプル**

```java
import android.view.View.OnClickListener;

public class Test1Activity extends AppCompatActivity implements OnClickListener {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
      Button button1 = (Button)findViewById(R.id.Button1);
      Button button2 = (Button)findViewById(R.id.Button2);
      
      // イベントの処理先をthis(アクティビティ)に設定
      button1.setOnClickListener(this);
      button2.setOnClickListener(this);
    }
    
    /**
     * アクティビティのイベント処理
     */
        @Override
    public void onClick(View v) {
      // 押されたWidgetをIdで判定する
      switch(v.getId()) {
          case R.id.button1:
              Log.v("button1 Clicked");
              break;
          case R.id.button2:
              Log.v("button2 Clicked");
              break;
      }
}
```

###テクニック

**id値を取得する**

```java
Int id = view.getId();
```

**id名を取得する**
整数のID値から文字列のid名を取得する

```java
String id = getApplicationContext().getResources().getResourceEntryName(id値);
```

##各種イベント
###クリック
クリックされた時に発生するイベント。View系のみ。ViewGroupは発生しない。

```java
import android.view.View.OnClickListener;

public class Test1Activity extends AppCompatActivity implements OnClickListener{
    public static final String TAG = "myLog";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
      ・
      ・
      ・
      button1.setOnClickListener(this);
    }
    
    @Override
    public void onClick(View v) {
        // 押されたWidgetをIdで判定する
        switch(v.getId()) {
            case R.id.button1:
                break;
        }
    }
}

```

###タッチ
タッチイベント(タッチ開始、タッチ終了、タッチ中の移動)。View,ViewGroupで発生する。

```java
import android.view.View.OnTouchListener;

public class Test1Activity extends AppCompatActivity implements OnTouchListener{
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    ・
    ・
    ・
    frame1.setOnTouchListener(this);
  }
  @Override
  public boolean onTouch(View v, MotionEvent e){
    String action = "";

    boolean ret = false;
    switch(e.getAction()) {
      case MotionEvent.ACTION_DOWN:
          action = "ACTION_DOWN";
          ret = true;   // MOVEイベントを発生させる場合はtrueを返す
          break;
      case MotionEvent.ACTION_UP:
          action = "ACTION_UP";
          break;
      case MotionEvent.ACTION_MOVE:
          action = "ACTION_MOVE";
          break;
      case MotionEvent.ACTION_CANCEL:
          action = "ACTION_CANCEL";
          break;
      default:
          action = "" + e.getAction();
    }

    String id = getApplicationContext().getResources().getResourceEntryName(v.getId());
    Log.v(TAG, "action:" + action + " id:" + id + " x:" + e.getX() + " y:" + e.getY());
    return ret;
  }
}
```

###ドラッグ
Viewをドラッグした時の処理。注意点としては、親Viewの中にある子Viewをドラッグしたい場合は、親Viewにイベントリスナを登録しないと正しいドラッグ座標が取れない。

```java
public class DragActivity extends AppCompatActivity implements View.OnTouchListener, View.OnDragListener {
    private ImageView mImageView;
    private View mDragView;
    private RelativeLayout mContainer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_drag);

        mContainer = (RelativeLayout)findViewById(R.id.container1);
        mImageView = (ImageView)findViewById(R.id.imageView2);

        // イベントリスナを設定
        mContainer.setOnDragListener(this);
        mImageView.setOnTouchListener(this);
    }

    /**
     * タッチイベント
     */
    @Override
    public boolean onTouch(View v, MotionEvent e){

        switch(e.getAction()) {
            case MotionEvent.ACTION_DOWN:
            {
                // ドラッグ開始
                mDragView = v;
                mDragView.startDrag(null, new View.DragShadowBuilder(v), null, 0);
            }
            break;
        }
        return true;
    }

    /**
     * ドラッグイベント
     */
    public boolean onDrag(View v, DragEvent e) {
        String action = "";
        switch(e.getAction()) {
            case DragEvent.ACTION_DROP: // ドロップ時
                action = "ACTION_DROP";
            {
                // ImageViewをドロップ先に移動
                ViewGroup.MarginLayoutParams layout = (ViewGroup.MarginLayoutParams)mDragView.getLayoutParams();
                layout.leftMargin = (int)e.getX() - mDragView.getWidth()/2;
                layout.topMargin = (int)e.getY() - mDragView.getHeight()/2;
                mDragView.setLayoutParams(layout);
                Log.v("mylog", "x:" + e.getX() + " y:" + e.getY());
            }    break;

            case DragEvent.ACTION_DRAG_ENDED: //ドラッグ終了
                action = "ACTION_DRAG_ENDED";

                break;

            case DragEvent.ACTION_DRAG_LOCATION: //ドラッグ中
                action = "ACTION_DRAG_LOCATION";
                Log.v("mylog", "x:" + e.getX() + " y:" + e.getY());
                break;

            case DragEvent.ACTION_DRAG_STARTED:  //ドラッグ開始時
                action = "ACTION_DRAG_STARTED";
                break;

            case DragEvent.ACTION_DRAG_ENTERED: //ドラッグ開始直後
                action = "ACTION_DRAG_ENTERED";
                break;

            case DragEvent.ACTION_DRAG_EXITED: //ドラッグ終了直前
                action = "ACTION_DRAG_EXITED";
                break;

            default:
        }
        Log.v("myLog", action);

        return true;
    }
}
```

###長押し
長押しイベント

```java
public class MainActivity extends AppCompatActivity implements OnDragListener, OnLongClickListener {
  
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    ・
    ・
    ・
    // イベントリスナーを登録
    button.setOnLongClickListener(this);
  }
  
  @Override
  public boolean onLongClick(View v) {
    Log.v("myLog", "long clicked");
  }
}
```