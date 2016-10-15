#HTTP通信

AndroidでHTTP通信を行うには HttpURLConnection クラスを使用する。HttpClientは deprecated なので使用しない方が良い。

* HTTP通信を行うには `INTERNET` パーミッションが必要
* 通信処理はUIスレッドで行うことはできない。AsyncTaskでスレッドを立ち上げて処理を行う。

AndroidManifest.xml に パーミッションを追加する

```java
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.shutaro.testhttp">
    <uses-permission android:name="android.permission.INTERNET" />
  ・
  ・
  ・
```

### AsyncTaskを使う
http通信はメインスレッド(UIスレッド)では実行できないため、AsyncTaskでバックグラウンで処理する。テンプレートはこんな感じ。バックグラウンドで処理したいタスクにStringを渡している。

```java
new AsyncTask<String, Void, String>() {
  /**
   * 通信において発生したエラー
   */
  private Throwable mError = null;

  @Override
  protected String doInBackground(String... params) {
      // バックグラウンドで行いたい処理
      // 引数は params[0] で取得する
      return null;
  }

  @Override
  protected void onPreExecute() {
      super.onPreExecute();
      // doInBackground前処理
  }

  @Override
  protected void onPostExecute(String result) {
      super.onPostExecute(result);
      // doInBackground後処理
  }
}.execute("hoge");
```

###runOnUiThread
バックグラウンド処理の中でUIにアクセスしようとすると
`android.view.ViewRootImpl`
エラーが起こる。バックグラウンドからUIを参照する場合は runOnUiThread でスレッドを作成して、そこからアクセスする。

```java
// mTextViewに文字列を追加する処理
private void sendText(final String text) {
    runOnUiThread(new Runnable() {
        @Override
        public void run() {
          // ここでUIにアクセスする
          mTextView.append(text);
        }
    });
}
```

### HttpURLConnection
通信処理はHttpURLConnectionクラスを使用する。

###最もシンプルな通信処理
指定のURLにhttp接続してページの出力を表示する。

```java
try {
  // 接続用HttpURLConnectionオブジェクト作成
  HttpURLConnection con = (HttpURLConnection)url.openConnection();
  // リクエストメソッドの設定(GET or POST)
  con.setRequestMethod("GET");
  // リダイレクトを自動で許可しない設定
  con.setInstanceFollowRedirects(false);
  // ヘッダーの設定(複数設定可能)
  con.setRequestProperty("Accept-Language", "jp");

  // 接続
  con.connect();

  // ヘッダの取得
  sendText("Response Code:" + con.getResponseCode() + "\n");

  Map headers = con.getHeaderFields();
  Iterator headerIt = headers.keySet().iterator();
  String header = null;
  while(headerIt.hasNext()){
      String headerKey = (String)headerIt.next();
      header += headerKey + "：" + headers.get(headerKey) + "\r\n";
  }
  if (header != null) {
      sendText(header);
  }

  // 本文の取得
  InputStreamReader ir = new InputStreamReader(
          con.getInputStream(),"utf-8");
  BufferedReader br = new BufferedReader(ir);
  String line = null;
  StringBuilder sb = new StringBuilder();
  while ((line = br.readLine()) != null) {
      sb.append(line + "\n");
  }
  br.close();

  sendText(sb.toString());
} catch (Exception e) {
  sendText(e.toString());
}
```

####POSTメッセージを送信する
POSTでメッセージを送信して、ページからの出力を表示する。

```java
HttpURLConnection con = null;
String postUrl = "http://sunsunsoft.com/test/android/test_post.php";
// Postで送信するパラメータ
String params = "param1=value1&param2=value2";

try {
    URL url = new URL(postUrl);

    // 接続用HttpURLConnectionオブジェクト作成
    con = (HttpURLConnection)url.openConnection();
    // リクエストメソッドの設定
    con.setRequestMethod("POST");
    con.setDoInput(true);
    con.setDoOutput(true);
    // リダイレクトを自動で許可しない設定
    con.setInstanceFollowRedirects(false);
    // ヘッダーの設定(複数設定可能)
    con.setRequestProperty("Accept-Language", "jp");
    con.setRequestProperty("Content-Type", "application/x-www-form-urlencoded;");
    // 接続
    con.connect();

    // データを送信する
    OutputStreamWriter out = new OutputStreamWriter(con.getOutputStream());
    out.write(params);
    out.close();

    // ヘッダの取得
    Map headers = con.getHeaderFields();
    Iterator headerIt = headers.keySet().iterator();
    String header = null;
    while(headerIt.hasNext()){
        String headerKey = (String)headerIt.next();
        header += headerKey + "：" + headers.get(headerKey) + "\r\n";
    }
    if (header != null) {
        sendText(header);
    }

    // 本文の取得
    InputStreamReader ir = new InputStreamReader(
            con.getInputStream(),"utf-8");
    BufferedReader br = new BufferedReader(ir);
    String line = null;
    StringBuilder sb = new StringBuilder();
    while ((line = br.readLine()) != null) {
        sb.append(line + "\n");
    }
    br.close();

    sendText(sb.toString());
} catch (Exception e) {
    sendText(e.toString());
}
```

対応するPHPソース

```php
-test_post.php-

<?php

echo "POST message\n";

// POSTメッセージを全部表示
foreach($_POST as $key=>$value) {
    echo "key=${key} value=${value}\n";
}

?>
```

####JSONをPOSTする
JSON形式のデータをPOSTし、ページからの出力を取得する。

```java
HttpURLConnection con = null;

// URLの作成
try {
    URL url = new URL("http://sunsunsoft.com/test/android/test_json.php");

    sendText(json.toString());

    // 接続用HttpURLConnectionオブジェクト作成
    con = (HttpURLConnection) url.openConnection();
    // リクエストメソッドの設定
    con.setRequestMethod("POST");
    con.setDoInput(true);
    con.setDoOutput(true);
    // リダイレクトを自動で許可しない設定
    con.setInstanceFollowRedirects(false);
    // ヘッダーの設定(複数設定可能)
    con.setRequestProperty("Accept-Language", "jp");
    con.setRequestProperty("Content-Type", "application/json; charset=UTF-8");

    // 接続
    con.connect();

    // データを送信する
    OutputStreamWriter out = new   OutputStreamWriter(con.getOutputStream());
    out.write(json.toString());
    out.close();

    // ヘッダの取得
    Map headers = con.getHeaderFields();
    Iterator headerIt = headers.keySet().iterator();
    String header = null;
    while (headerIt.hasNext()) {
        String headerKey = (String) headerIt.next();
        header += headerKey + "：" + headers.get(headerKey) + "\r\n";
    }
    if (header != null) {
        sendText(header);
    }

    // 本文の取得
    InputStreamReader ir = new InputStreamReader(
            con.getInputStream(),"utf-8");
    BufferedReader br = new BufferedReader(ir);
    String line = null;
    StringBuilder sb = new StringBuilder();
    while ((line = br.readLine()) != null) {
        sb.append(line + "\n");
    }
    br.close();

    sendText(sb.toString());
} catch (Exception e) {
    sendText(e.toString());
}
```

対応するphpのソース

```php
<?php
//リクエストの body 部から生のデータを読み込む
$input = file_get_contents('php://input');

//JSON のデコード
$data = json_decode($input);

//以下確認用
header('Content-type: text/plain; charset=utf-8');

foreach($data as $key => $val) {
    echo $key.' = '.$val."\n";
}
?>
```

###httpで画像を読み込む
![](http://sunsunsoft.com/image/android/http_download_image.png)
HttpURLConnection で 画像をダウンロードする方法。

```java
private Bitmap downloadImage(String imageUrl) {
    // 受け取ったbuilderでインターネット通信する
    HttpURLConnection connection = null;
    InputStream inputStream = null;
    Bitmap bitmap = null;

    try{

        URL url = new URL(imageUrl);
        connection = (HttpURLConnection)url.openConnection();
        connection.setRequestMethod("GET");
        connection.connect();
        inputStream = connection.getInputStream();

        bitmap = BitmapFactory.decodeStream(inputStream);
    }catch (MalformedURLException exception){

    }catch (IOException exception){

    }finally {
        if (connection != null){
            connection.disconnect();
        }
        try{
            if (inputStream != null){
                inputStream.close();
            }
        }catch (IOException exception){
        }
    }

    return bitmap;
}
```

