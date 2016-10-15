#ファイル操作 File


##外部ストレージに保存する
にはmanifestファイルに以下の記述を追加する

```java
- AndroidManifest.xml

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.shutaro.testscreenshot">

    <!-- 外部ストレージに保存する permissio　を追加 -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
    
    <application
```

##ディレクトリ取得

|メソッド|場所|例|
|---|---|---|
|getExternalStorageDirectory()|外部ストレージ|/storage/emulated/0/|
|getExternalFilesDir<br>(Environment.DIRECTORY_PICTURES)|Picturesディレクトリ|/storage/emulated/0//Android/data/<パッケージ名>/pictures/|
|getDownloadCacheDirectory()|ダウンロードディレクトリ|/cache/|
|getExternalStoragePublicDirectory<br>(Environment.DIRECTORY_DOCUMENTS)|ドキュメントディレクトリ|/storage/emulated/0/Documents/|
|getRootDirectory() ※1|Systemルートディレクトリ|/system/|

※1  root以下はread only なので書き込みはできない

##ファイル書き込み
AndroidManifest.xml に パーミッションが追加されていることを確認。

FileWrite オブジェクトを作成してから、write で文字列を書き込む。

```java
/**
 * 外部ストレージ/temp/test.txt に文字列を書き込む 
 */
private String writeContents(String contents) {
  File temppath = new File(Environment.getExternalStorageDirectory(),"temp");

  if (temppath.exists() != true) {
      temppath.mkdirs();
  }

  File tempfile = new File(temppath, "test.txt");
  FileWriter output = null;
  try {
      output = new FileWriter(tempfile, true);
      output.write(contents);
      output.write("\n");
  } catch (FileNotFoundException e) {
      e.printStackTrace();
  } catch (IOException e) {
      e.printStackTrace();
  } finally {
      if (output != null) {
          try {
              output.close();
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
  }
}
```

##ファイル読み込み
InputStreamクラスかReaderクラスの子クラスでファイルを読み込むことができる。
以下のサンプルではBufferReader オブジェクト経由でファイルを読み込む。

```java
/**
 * ファイルを読み込む
 * @param path 読み込むファイルのパス 例: "/storage/emulated/0/temp/test.txt"
 */
private void readFile(String path) {
    try {
        File file = new File(path);
        readText = file.getName() + "\n";
        BufferedReader bufferReader = new BufferedReader(new FileReader(
                file));
        String StringBuffer;
        String stringText = "";
        while ((StringBuffer = bufferReader.readLine()) != null) {
            stringText += StringBuffer;
        }
        bufferReader.close();
        readText += stringText;
    } catch (MalformedURLException e) {
        e.printStackTrace();
        readText += e.toString();
    } catch (IOException e) {
        e.printStackTrace();
        readText += e.toString();
    }
    textView.setText(readText);
}
```

##アプリの設定値を保持する
SharedPreferencesオブジェクトを取得

```java
// SharedPreferences.Editor オブジェクトを取得
public SharedPreferences getSharedPreferences(String name, int mode)
```

```java
###書き込み
```

```java
SharedPreferences.Editor editor = SharedPreferencesオブジェクト.edit()
```

あとはこのSharedPreferences.Editorオブジェクトを使ってデータの書き込みを行う。

```java
SharedPreferences pref = getSharedPreferences("config1", Context.MODE_PRIVATE);
SharedPreferences.Editor editor = pref.edit();

editor.putString("editConfigText", mEditConfigText.getText().toString());
editor.putBoolean("checkConfigCheck1", mCheckConfigCheck1.isChecked());
editor.putBoolean("checkConfigCheck2", mCheckConfigCheck2.isChecked());
editor.putInt("spinnerConfigSelect", mSpinnerConfigSelect.getSelectedItemPosition());

editor.commit();
```

###読み込み
SharedPreferencesオブジェクトのget~系のメソッドを使ってデータを読み込む。

```java
SharedPreferences pref = getSharedPreferences("config1",Context.MODE_PRIVATE);

pref.getString("keyText1", "default str");
pref.getBoolean("keyBool1",false));
pref.getInt("keyInt1", 0);
```

SharedPreferencesメソッド

|メソッド名|説明|
|---|---|
|getString(String key, String default_value)|文字列を読み込む
|getBoolean(Strng key, Boolean default_value)|Bool値を読み込む
|getInt(String key, Int default_value)|整数値を読み込む|
|Map<String, ?> getAll()|全てのデータを読み込む|

Editorインターフェイス

|メソッド名|説明|
|---|---|
|remove(String key)|指定したキーのデータを削除する|
|clear()|全てのデータを削除する|
|putString(String key, @Nullable String value)|文字列を追加する|
|putBoolean(String key, boolean value)|bool値を追加する|
|putInt(String key, int value)|整数値を追加する|
|commit()|書き込みを確定する|

##キャッシュディレクトリ
各アプリ用のキャッシュフォルダは以下のメソッドで取得できる。

File internalCachedir = getCacheDir();
File externalCachedir = getExternalCacheDir();

これらのでメソッドで

```java
// 内部ストレージキャッシュのパスを表示
File internalCachedir = getCacheDir();
Log.v("myLog", internalCachedir.getPath());
File filePath = new File(internalCachedir.getPath(), "test1.txt");

// 外部ストレージ(SDカード等)キャッシュのパスを取得 
File externalCachedir = getExternalCacheDir();
Log.v("myLog", externalCachedir.getPath());
File filePaht2 = new File(externalCachedir.getPath(), "test1.txt"); 
```