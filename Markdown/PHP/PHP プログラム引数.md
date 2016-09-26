###プログラムの引数
コマンドラインから

```sh
$ php test.php arg1 arg2 
```

を実行した場合、$argv に引数が入る

$argv[0]  "test.php"
$argv[1]  "arg1"
$argv[2]  "arg2"

```php
// プログラムの引数を全て表示する
for ($args as $arg) {
  print($art . "\n");
}

```