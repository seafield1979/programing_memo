#ファイル操作 File


##外部ストレージに保存する
にはmanifestファイルに以下の記述を追加する

```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.shutaro.testscreenshot">

    <!-- 外部ストレージに保存する permissio　を追加 -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
    
    <application
```

##ディレクトリ取得

|メソッド|場所|
|---|---|
|getExternalStorageDirectory()||/storage/emulated/0|
|getExternalFilesDir(Environment.DIRECTORY_PICTURES)|/storage/emulated/0//Android/data/<パッケージ名>/pictures|