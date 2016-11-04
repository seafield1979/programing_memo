#クリップ

###リンク
  
[愚鈍人 グラフィックス（９）-描画領域のクリップ](http://ichitcltk.hustle.ne.jp/gudon2/index.php?pageType=file&id=Android042_Graphics9_clip)

##クリッピング

Canvas#clipRect() メソッドを使うと、クリップ領域外の描画がされなくなる。
クリップ領域は変更できる

```java
public void onDraw(Canvas canvas) {
    // クリッピング前の状態を保存
    canvas.save();

    // クリッピングを設定
    canvas.clipRect(50, 50, 400, 400);

    myDrawCircle(canvas, paint, 150, 150, 200, Color.rgb(0,250,128));
    myDrawCircle(canvas, paint, 50, 50, 100, Color.rgb(128,0,128));

    // クリッピングを解除
    canvas.restore();
}
```

```java
// 複数のクリッピイング
canvas.save();

Path path = new Path();
path.addCircle(100, 60, 50, Path.Direction.CCW);
path.addCircle(60, 140, 50, Path.Direction.CCW);
path.addCircle(140, 140, 50, Path.Direction.CCW);
canvas.clipPath(path);

myDrawRect(canvas, paint, 0,0, 1000, 1000, Color.rgb(120, 50, 200));

canvas.restore();

```