#正規表現 regexp
[preg_match](http://php.net/manual/ja/function.preg-match.php)
正規表現を使用したパターンマッチ。マッチした文字列を取得することもできる。

```php
preg_match(パターン, 対象文字列, 結果が格納される変数);

戻り値: マッチしたら1 マッチしなかったら0
```

~~~php
preg_match('/(.+)\.js/', `$src_string`, $m);
$m[0] -> ヒットした文字列全体
$m[1] -> １つ目の()にヒットした文字列
$m[2] -> ２つ目の()にヒットした文字列
~~~

```php
preg_match('/(\w+)(\s+)/', $str, $m);
if (count($m) == 3) {
  // マッチした
  print($m[0] . " " . $m[1] . " " . $m[2] . "\n");
}


```

```php
// 英数半角文字チェック
$str = "hogehoge";
if (preg_match("/^[a-zA-Z0-9]+$/", $str)) {
  echo "すべて半角英数である";
} else {
  echo "すべて半角英数ではない";
}

```