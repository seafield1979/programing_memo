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
    </LinearLayout>
</HorizontalScrollView>


```