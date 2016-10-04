#Bitmap

###Bitmapを読み込む
リソースからBitmapを生成する

```java
Bitmap BitmapFactory.decodeResource(Resources res, int id);

Bitmap bmp = BitmapFactory.decodeResource(getResources(), R.drawable.robot2);
```

###Bitmapから拡大縮小

```java
Bitmap Bitmap.createScaledBitmap(bmp, width, height, filter);

Bitmap newBmp = Bitmap.createScaledBitmap(bmp, 200, 90, false);
```