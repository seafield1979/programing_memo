#Cookie
WebViewでCookieを操作したい場合はCookieManagerを使う。

以下、DeveloperのCookieManagerのページ。メソッドリストがある。
[Android Developer CookieManager](https://developer.android.com/reference/android/webkit/CookieManager.html)

CookieManagerオブジェクトはCookieManager.getInstance() で取得する。newはしないこと。
古いサンプルでは CookieSyncManager を使用してアクティビティーのサスペンドレジューム時にいろいろ処理をしているが、API21以降のWebViewでは必要なくなった。

|戻り値|メソッド|説明|
|---|---|---|
|CookieManager|getInstance()|クラスメソッド、CookieManagerオブジェクトを取得する|
|String<br>Cookieの文字列|getCookie(String url)|cookieを取得。urlに設定するURLは"http://www.google.co.jp" とか|
|void |setCookie(String url, String cookie)|Cookieを設定する。cookieは"key=value"形式
|void|removeAllCookies(ValueCallback<Boolean> callback)|全Cookieを削除する|
|boolean|hasCookies()|Cookieが保存されているかをチェックする|


###Cookieを取得

```java
String url = "http://www.sunsunsoft.com";

CookieManager cm = CookieManager.getInstance();
String cookieStr = cm.getCookie(url);
if (cookieStr != null) {
    String[] cookies = cookieStr.split(";");
    for (String cookie : cookies) {
        mTextView.append(cookie + "\n");
    }
}
```

###Cookieを設定

```java
String url = "http://www.sunsunsoft.com";
String cookies = "key1=value1;key2=value2";

CookieManager cm = CookieManager.getInstance();
for (String cookie : cookies.split(";")) {
    cm.setCookie(url, cookie);
}
cm.flush();
```

###Cookieを削除

```ini
CookieManager cm = CookieManager.getInstance();
cm.removeAllCookies(new ValueCallback<Boolean>() {
    @Override
    public void onReceiveValue(Boolean value) {
        mTextView.append("onReceiveValue " + value + "\n");
    }
});
```