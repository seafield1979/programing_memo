#Cookie
Webページを表示した場合に、ブラウザにCookieを保存しておき次回以降ページを表示した時にそのCookieの値を参照することができる。  
※ChromeではローカルファイルでCookieの保存ができない。

##Cookie基本操作
###Cookieを保存
~~~php
setcookie( cookieName , value , [timeout] , [path] , [domain] )
setcookie( "hoge", 123, time() + (3*(24*60*60)));
~~~

|引数名|説明|
|---|---|
|cookieName | Cookie名
|value |      Cookie値
|timeout |  ※省略可能。Cookieの有効期限。このCookieが有効となる期限を秒単位で指定する<br>例）0 … ブラウザが閉じられた時点で削除される<br>例）time() + (30 * (24*60*60)) … 30日間有効になります。<br>time()で、現在時刻が『1970年1月1日 00:00:00 GMT』からの通算秒として返されます。<br>そこに30日×(24時間×60時間×60秒)を加算して算出しています。
|path | ※省略可能。Cookieが有効なパス。<br>例）"/"
|domain |  ※省略可能<br>Cookieが有効なドメイン。<br>例）"www.24w.jp"

###Cookieを取得

```php
var value = $_COOKIE[ cookieName ]
$hoge = $_COOKIE["hoge"];
```

###Cookieを削除

```php
setcookie( "クッキー名", "", time()-10000);
// timeに過去の日付を設定すると削除される。
```