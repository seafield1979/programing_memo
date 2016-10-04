#ImageView


レイアウトで画像を表示する

```xml
<ImageView
    android:layout_width="100dp"
    android:layout_height="100dp"
    android:id="@+id/imageView"
    android:background="@drawable/robot2"
    android:scaleType="matrix"/>
    
```

###Bitmapを表示する
画像ファイル(png,jpg等)をImageViewに表示する

```java
ImageView iv = (ImageView)findViewById(R.id.imageView);

// drawableの画像からBitmapを作成
Bitmap bmp = BitmapFactory.decodeResource(getResources(), R.drawable.robot2);

// ImageViewにBitmapを表示
iv.setImageBitmap(bmp);
```

###画像をスケールして表示

```java
ImageView iv2 = (ImageView)findViewById(R.id.imageView2);
ImageView iv3 = (ImageView)findViewById(R.id.imageView3);

// drawableの画像からBitmapを作成
Bitmap bmp = BitmapFactory.decodeResource(getResources(), R.drawable.robot2);

// 100x200 pixで表示
Bitmap bmp2 = Bitmap.createScaledBitmap(bmp, 100, 200, false);
iv2.setImageBitmap(bmp);

// 200x200 pix で表示
Bitmap bmp3 = Bitmap.createScaledBitmap(bmp, 200, 200, false);
iv2.setImageBitmap(bmp);
```