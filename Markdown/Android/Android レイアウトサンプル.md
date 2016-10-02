#Layout Samples

##RelativeRayout

###指定Viewの上下左右に配置
![](http://sunsunsoft.com/image/android/layout_relative.png)

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_below="@+id/button4"
    >

  <Button
      android:layout_width="50dp"
      android:layout_height="wrap_content"
      android:text="C"
      android:id="@+id/buttonC"
      android:layout_centerVertical="true"
      android:layout_centerHorizontal="true"/>
  
  <Button
      android:layout_width="50dp"
      android:layout_height="50dp"
      android:text="U"
      android:id="@+id/buttonU"
      android:layout_above="@+id/buttonC"
      android:layout_alignStart="@+id/buttonC"
      android:layout_marginBottom="20dp"/>
  
  <Button
      android:layout_width="50dp"
      android:layout_height="50dp"
      android:text="D"
      android:id="@+id/buttonD"
      android:layout_below="@+id/buttonC"
      android:layout_alignLeft="@+id/buttonC"
      android:layout_marginTop="20dp"/>
  
  <Button
      android:layout_width="50dp"
      android:layout_height="50dp"
      android:text="L"
      android:id="@+id/button7"
      android:layout_toLeftOf="@+id/buttonC"
      android:layout_alignTop="@+id/buttonC"
      android:layout_marginRight="20dp"/>
  
  <Button
      android:layout_width="50dp"
      android:layout_height="50dp"
      android:text="R"
      android:id="@+id/buttonR"
      android:layout_toRightOf="@+id/buttonC"
      android:layout_alignTop="@+id/buttonC"
      android:layout_marginLeft="20dp"/>

</RelativeLayout>
```

###指定Viewと同じ位置に配置
layouot_toAlign系
![](http://sunsunsoft.com/image/android/layout_toAlign.png)

```xml
<RelativeLayout android:id="@+id/area1"
        android:layout_width="match_parent"
        android:layout_weight="1"
        android:layout_height="200dp"
                android:background="#b9d4da">

    <Button
        android:layout_width="120dp"
        android:layout_height="50dp"
        android:text="Top"
        android:id="@+id/button5"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true"/>

    <Button
        android:layout_width="120dp"
        android:layout_height="50dp"
        android:text="Left"
        android:id="@+id/button6"
        android:layout_centerVertical="true"
        android:layout_alignParentStart="true"/>

    <Button
        android:layout_width="120dp"
        android:layout_height="50dp"
        android:text="Right"
        android:id="@+id/button8"
        android:layout_alignTop="@+id/button6"
        android:layout_alignParentEnd="true"/>

    <Button
        android:layout_width="120dp"
        android:layout_height="50dp"
        android:text="bottom"
        android:id="@+id/button9"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="layout_alignParent~"
        android:id="@+id/textView10"
        android:layout_alignParentTop="true"
        android:layout_alignParentStart="true"/>
</RelativeLayout>
```

###親の上下左右端に配置
![](http://sunsunsoft.com/image/android/layout_alignParent.png)
layout_alignParent系

```xml
<RelativeLayout android:id="@+id/area2"
        android:layout_width="match_parent"
                android:layout_height="188dp"
                android:background="#debfbf">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="base"
        android:id="@+id/button10"
        android:layout_marginStart="27dp"
        android:layout_marginTop="26dp"
        android:layout_alignParentTop="true"
        android:layout_alignParentStart="true"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Left"
        android:id="@+id/button11"
        android:layout_below="@+id/button10"
        android:layout_alignStart="@+id/button10"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Top"
        android:id="@+id/button12"
        android:layout_alignTop="@+id/button10"
        android:layout_toEndOf="@+id/button10"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="base"
        android:id="@+id/button13"
        android:layout_alignParentEnd="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="20dp"
        android:layout_marginBottom="20dp"
        />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Right"
        android:id="@+id/button14"
        android:layout_above="@+id/button13"
        android:layout_alignEnd="@+id/button13"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Bottom"
        android:id="@+id/button15"
        android:layout_alignBottom="@+id/button13"
        android:layout_toStartOf="@+id/button13"/>
</RelativeLayout>
```

###親Viewの水平、垂直の中央に配置
![](http://sunsunsoft.com/image/android/layout_center.png)
layout_center系

```xml
<RelativeLayout android:id="@+id/area1"
                android:layout_width="match_parent"
                android:layout_height="125dp"
                android:background="#b9d4da">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="CenterH"
        android:id="@+id/button16"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="CenterV"
        android:id="@+id/button17"
        android:layout_centerVertical="true"
        android:layout_alignParentStart="true"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Center/Center"
        android:id="@+id/button18"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="layout_center~"
        android:id="@+id/textView11"
        android:layout_alignParentTop="true"
        android:layout_alignParentStart="true"/>
</RelativeLayout>
```

