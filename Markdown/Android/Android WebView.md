#WebView

使い方はiOSのUIWebViewとだいたい同じ感じ。


[Android Developer WebView](https://developer.android.com/reference/android/webkit/WebView.html)

**WebViewメソッド**

|戻り値|メソッド名、引数|説明|
|---|---|---|
|WebSettings|getSettings()|WebSettingsオブジェクトを取得する。このオブジェクトを使用して各種設定(JavaScript ON/OFF),UserAgent等）を行う|
|void|setWebViewClient(WebViewClient client)|WebView内で新しいページを表示するようにする|
|void|setWebChromeClient(WebChromeClient client)|各種状態を取得するための進捗状態を取得する|
|void|loadUrl(String url)|Webページを表示する|
|void|goBack()|前のページに戻る|
|boolean|canGoBack()|前のページに戻れるかをチェックする|
|void|goForward()|次のページに進む(goBackで戻ったあと、再度元のページを表示させる)|
|void|reload()|ページを再読み込みする|

**WebSettingsメソッド**
かなり大量にメソッドがある。実際に使う時に必要な機能を探すのがよいかもしれない。
[Android Developer WebSettings](https://developer.android.com/reference/android/webkit/WebSettings.html)

|戻り値|メソッド名、引数|説明|
|---|---|---|
|String|getUserAgentString()|UserAgentを取得する|
|void| setUserAgentString(String ua)|UserAgentを設定する|
|void|setJavaScriptEnabled(boolean flag)|JavaScriptの有効/無効を設定する|
|void|setBuiltInZoomControls(boolean flag)|デフォルトのズームコントロールの有効/無効を設定する|


###簡単な使用方法

マニフェストファイルにパーミッションを追加する

AndroidManifest.xml

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

WebViewオブジェクト生成
onCreateあたりで以下のコードを実行。setWebViewClientを設定しないとloadUrl()で標準のWebブラウザで開いてしまう。

```java
WebView webview = new WebView(this);  
// アプリ内でページを開く
webview.setWebViewClient(new WebViewClient() {});  
```

ページを読み込む

```java
webview.loadUrl("http://www.google.com");  
```

###ページ読み込みの進捗を表示する

```java
webView.setWebChromeClient(new WebChromeClient(){
    @Override
    public void onProgressChanged(WebView view, int newProgress) {
        // newProgressに0~100までの進捗値が入る
        super.onProgressChanged(view, newProgress);
        // progress barに表示したりする
    }
});
```

###WebViewのメソッド

```java
/**
 * 設定系
 */

// UserAgentを設定する
String ua = "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/28.0.1500.63 Safari/537.36";
webView.getSettings().setUserAgentString(ua);

// JavaScriptを有効にする
webview.getSettings().setJavaScriptEnabled(true);  

// デフォルトのズームコントロールを使用
webview.getSettings().setBuiltInZoomControls(true);  

/**
 * アクション系
 */
// ページをリロード
webView.reload();

// 前のページに戻る
webView.goBack();

// 元のページ(戻る前のページ)に進む
webView.goForward();
```

###端末のバックキーでページバックする
Activityクラスの onBackPressed メソッドをオーバーライドする

```java
@Override
public void onBackPressed() {
    if (mWebView.canGoBack()) {
        // ページバックできるなら戻る
        mWebView.goBack();
    } else {
        // できないなら通常の戻るボタンの処理
        super.onBackPressed();
    }
}
```

###ローカルhtmlファイルを表示する
Webの解説ページだとassetsフォルダ以下にhtmlファイルを置けって書いてあるがAndroid Studioで作成したプロジェクトだと、デフォルトでassetsフォルダは作られない。

なので、自分でフォルダを作成する。
`<プロジェクト>/app/main/src/assets`
を作ると自動でプロジェクトのソースツリーにassets が表示される。

![](http://sunsunsoft.com/image/android/add_assets.png)

![](http://sunsunsoft.com/image/android/add_assets2.png)

assetsフォルダに適当なhtmlファイルを配置して、loadUrl("file:///android_asset~")で読み込む。

```java
// loadUrl("file:///android_asset/htmlファイル名");
mWebView.loadUrl("file:///android_asset/hoge.html");
```