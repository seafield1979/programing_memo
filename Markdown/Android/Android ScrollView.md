#ScrollView

ScrollViewで囲んだViewが領域に収まりきらなくなるとスクロールできるようになる。
が、全てのViewがスクロールできるわけではなく、LinearLayout等のViewGroup系のViewしかスクロールできないようだ。

```xml
縦方向にスクロールさせる場合
<ScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/scrollView"
    >

    <LinearLayout
        android:id="@+id/linearMain"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
      
        <!-- ここにスクロールするWidgetを挿入 -->

    </LinearLayout>
</ScrollView>

横方向にスクロールさせる場合の例
<HorizontalScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <LinearLayout
        android:id="@+id/linearMain"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
      
      <!-- ここにスクロールするWidgetを挿入 -->
      
    </LinearLayout>
</HorizontalScrollView>
```

###スクロールを停止する方法
ScrollViewの子クラスのイベント処理で

```java
View.getParent().requestDisallowInterceptTouchEvent(true);
```

で親のScrollViewのスクロールを止めることができる。
停止続けたい間ずっと上記の処理を呼び続ける必要がある。

```java
public boolean onTouch(View v, MotionEvent e) {

switch(e.getAction()) {
    case MotionEvent.ACTION_DOWN:
        v.getParent().requestDisallowInterceptTouchEvent(true);
        break;
    case MotionEvent.ACTION_UP:
        v.getParent().requestDisallowInterceptTouchEvent(true);
        break;
    case MotionEvent.ACTION_MOVE:
        break;
    default:
  }
}
```