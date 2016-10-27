#キャンバス Canvas

### ベースのプロジェクト
キャンバスを使用した図形や文字の描画

ファイル構成
~~~
MainActivity.java  SampleViewを表示する
SampleView.java    Canvasに描画する処理
~~~

SampleView.java

```java
package com.example.shutaro.testcanvas;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.Path;
import android.graphics.Typeface;
import android.view.View;

public class SampleView extends View{
    private Paint paint = new Paint();

    public SampleView(Context context) {
        super(context);
        setBackgroundColor(Color.WHITE);
    }

    @Override
    public void onDraw(Canvas canvas) {
        // 背景塗りつぶし
        canvas.drawColor(Color.WHITE);
        
        paint.setTextSize(80);

        // 色を設定
        paint.setColor(Color.rgb(255,0,0));

        // 太字
        paint.setTypeface(Typeface.DEFAULT_BOLD);
        paint.setFakeBoldText(true);

        // 色を設定
        paint.setColor(Color.rgb(0,0,255));
        canvas.drawText("Hello!", 50, 50, paint);
    }
}
```

MainActivity.java

```java
~~~

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        setContentView(new SampleView(this));  // <- 追加
    }
}

```

###テキスト描画
ViewクラスのonDrawの中で呼び出す

シンプルなテキスト表示

```java
canvas.drawText(<表示する文字列>, <x座標>, <y座標>, paint);

canvas.drawText("Hello!", 50, 50, paint);
```

いろいろなパラメータを設定

|メソッド|説明|
|---|---|
|setTextSize(size)|テキストの描画サイズを設定する|
|setTypeface(type)|文字の種類を設定する|
|setFaceBoldText(bool)|テキストを太文字にする（日本語対応）|
|setTextSkewX(slope)|テキストの傾きを設定|
|setColor(color)|テキスト（その他）の色を設定|

```java
// テキストのサイズを設定
paint.setTextSize(80);

// アンチエリアシング(境界のぼかし)
paint.setAntiAlias(true);

// 色を設定
paint.setColor(Color.rgb(255,0,0));

// 太字
paint.setTypeface(Typeface.DEFAULT_BOLD);
paint.setFakeBoldText(true);

// 斜体
paint.setTextSkewX(-0.25f);

canvas.drawText("Hello!", 50, 100, paint);

// 色を設定
paint.setColor(Color.rgb(0,255,0));
canvas.drawText("Hello!", 50, 200, paint);
```

###図形描画
線、四角形、円を描画

共通メソッド
Paintクラスのメソッドなので `paint.メソッド名` で使用する

|メソッド|説明|
|---|---|
|setAntiAlias(bool)|アンチエリアシングを行うかどうか|
|setStyle(Point.Style.<種類>|描画の種類(塗りの有無)|
|setStrokeWidth(stroke_w)|図形の線の太さ|
|setColor(color)|図形の色|

**線の描画**
drawLine(p1_x, p1_y, p2_x, p2_y, paint)
![](http://sunsunsoft.com/image/android/canvas_text1.png)

```java
// アンチエリアシング(境界のぼかし)
paint.setAntiAlias(true);
// 線の種類
paint.setStyle(Paint.Style.STROKE);
// 線の太さ
paint.setStrokeWidth(4);

// 線
canvas.drawLine(10, 20, 100, 40, paint);
```

**つながった線を描画**
moveTo -> lineTo -> lineTo -> ...drawPath
Path.moveTo(x,y)
Path.lineTo(x,y)
drawPath(Path, paint)

```java
// 連続した線
path.moveTo(0, 100);
path.lineTo(100, 100);
path.lineTo(100, 200);
path.lineTo(200, 200);
path.lineTo(200, 300);
paint.setColor(Color.rgb(0,100,255));
canvas.drawPath(path, paint);
```

**四角形の描画**
![](http://sunsunsoft.com/image/android/canvas_rect.png)
drawRect(p1_x, p1_y, p2_x, p2_y, paint)

```java
// アンチエリアシング(境界のぼかし)
paint.setAntiAlias(true);
// 線の種類
paint.setStyle(Paint.Style.STROKE);
// 線の太さ
paint.setStrokeWidth(10);
// 色
paint.setColor(Color.rgb(255,0,0));
canvas.drawRect(50, 50, 300, 300, paint);

// 内部を塗りつぶし
paint.setStyle(Paint.Style.FILL_AND_STROKE);
paint.setColor(Color.rgb(0,180,0));
canvas.drawRect(50, 350, 500, 500, paint);
```

**円の描画**
![](http://sunsunsoft.com/image/android/canvas_circle.png)
drawCircle(center_x, center_y, radius, paint);

```java
// アンチエリアシング(境界のぼかし)
paint.setAntiAlias(true);

// 線の種類
paint.setStyle(Paint.Style.STROKE);
// 線の太さ
paint.setStrokeWidth(10);
// 色
paint.setColor(Color.rgb(255,0,0));

canvas.drawCircle(300, 300, 100, paint);
```

###画像ファイル
画像ファイルを読み込んで使用する

プロジェクトに画像を追加するには res の drawable 以下のフォルダに画像ファイルを配置してあげればよい。

~~~
res
  + drawable
    + imoni_s.png    画像ファイル
~~~

```java
// res/drawable/imoni_s.png をBitmap形式で読み込む
Bitmap bmp = BitmapFactory.decodeResource(getResources(), R.drawable.imoni_s);

canvas.drawBitmap(bmp, 0, 0, paint);

// スケール（伸縮）
canvas.scale(0.5f, 0.5f);
canvas.drawBitmap(bmp, 0, 0, paint);

// 指定のサイズで描画
Rect src = new Rect(0,0,width, height);
Rect dst = new Rect(x,y,x+width,y+height);
canvas.drawBitmap(bmp, src, dst, paint);
```

###レイアウトにCanvasを配置
layout~.xml 内にonDrawで描画を行う自前のViewを配置する。やり方としてはViewを継承した独自のViewクラスを作成して、それをlayout.xmlに配置する。

![](http://sunsunsoft.com/image/android/canvas_into_layout.png)

layout.xml

```xml
<com.example.testlayoutcanvas1.TestView
        android:id="@+id/test_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

カスタムしたView
TextView.java

```java
package com.example.testlayoutcanvas1;
 
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.util.AttributeSet;
import android.view.View;
 
public class TestView extends View {
 
    Paint paint;
 
    public TestView(Context context, AttributeSet attrs) {
 
        super(context, attrs);
        paint = new Paint();
    }
 
    @Override
    protected void onDraw(Canvas canvas) {
        // ここに描画処理
    }
}
```