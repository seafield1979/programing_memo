#リダイレクト　redirect

```php
header('location: [パス]');
```

※html本体より前に送信しないといけない。よってheaderの前に echo 文で文字列を出力していたりすると失敗する