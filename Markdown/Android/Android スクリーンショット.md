#ScreenShot スクリーンショット

指定したViewを画像ファイルに保存する

getScreenBitmap が画面全体をスクリーンショット
getViewBitmap が指定したViewをスクリーンショット

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
  
  public void screenShot1() {
    // 画面全体をスクリーンショット button1の親View->画面全体
    Button button = (Button)findViewById(R.id.button1);
    Bitmap bmp = getScreenBitmap(button);
    if (bmp != null) {
        saveBitmapToSd(bmp);
    }
  }
  
  public void screenShot2(){
    // 指定のViewをスクリーンショット
    Button button = (Button)findViewById(R.id.button1);
    Bitmap bmp = getViewBitmap(button);
    saveBitmapToSd(bmp);
  }
  
  /**
   * スクリーンショットしたBitmapを作成する
   * @param view このviewの親Viewのスクリーンショットを撮る
   * @return Bitmap
   */
  public Bitmap getScreenBitmap(View view){
      return getViewBitmap(view.getRootView());
  }

  /**
   * 指定したViewのBitmapを作成する
   * @param view
   * @return Bitmap
   */
  public Bitmap getViewBitmap(View view){
      view.setDrawingCacheEnabled(true);
      Bitmap cache = view.getDrawingCache();
      if(cache == null){
          return null;
      }
      Bitmap bitmap = Bitmap.createBitmap(cache);
      view.setDrawingCacheEnabled(false);
      return bitmap;
  }

  /**
   * 外部ストレージのtestフォルダにBitmapを保存する
   * /storage/emulated/0/test
   * @param mBitmap
   */
  public void saveBitmapToSd(Bitmap mBitmap) {
      try {
          // sdcardフォルダを指定
          String saveDir = Environment.getExternalStorageDirectory().getPath() + "/test";
          // SD カードフォルダを取得
          File file = new File(saveDir);

          // フォルダ作成
          if (!file.exists()) {
              if (!file.mkdir()) {
                  Log.e("Debug", "Make Dir Error");
              }
          }

          // 日付でファイル名を作成　
          Date mDate = new Date();
          SimpleDateFormat fileName = new SimpleDateFormat("yyyyMMdd_HHmmss");
          String imgPath = saveDir + "/" + fileName.format(mDate) + ".jpg";

          // ファイル保存
          FileOutputStream fos;
          try {
              fos = new FileOutputStream(imgPath, true);
              mBitmap.compress(Bitmap.CompressFormat.JPEG, 100, fos);
              fos.close();

              // アンドロイドのデータベースへ登録
              // (登録しないとギャラリーなどにすぐに反映されないため)
              registAndroidDB(imgPath);

          } catch (Exception e) {
              Log.e("Debug", e.getMessage());
          }

          fos = null;
      } catch (Exception e) {
          Log.e("Error", "" + e.toString());
      }
  }

  /**
   * アンドロイドのデータベースへ画像のパスを登録
   * @param path 登録するパス
   */
  private void registAndroidDB(String path) {
      // アンドロイドのデータベースへ登録
      // (登録しないとギャラリーなどにすぐに反映されないため)
      ContentValues values = new ContentValues();
      ContentResolver contentResolver = MainActivity.this.getContentResolver();
      values.put(MediaStore.Images.Media.MIME_TYPE, "image/jpeg");
      values.put("_data", path);
      contentResolver.insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, values);
  }
}


```