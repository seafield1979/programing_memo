##ファイル操作関数

###カレントディレクトリを変更 chdir
bool chdir ( string $directory )  
PHP のカレントディレクトリを directory に変更します。


###[rename](http://php.net/manual/ja/function.rename.php)
ファイル名を変更

`bool rename ( string $oldname , string $newname [, resource $context ] )`

```php
// hoge.txt を hage.txt に変更
rename("hoge.txt", "hage.txt");
```

###[dirname](http://php.net/manual/ja/function.dirname.php)
親フォルダ名を取得 

`string dirname ( string $path [, int $levels = 1 ] )`

```php
// hoge.txt の親フォルダを表示する
$dir_name = dirname("./hoge.txt");
print($dir_name . "\n");
```

###[file](http://php.net/manual/ja/function.file.php)
テキストファイルの全行を配列に読み込む

`array file( string filename [, int use_include_path ] )`

```php
// hoge.txt を読み込んで全行出力
$list = file("hoge.txt");
for ($line as $list) {
  print($line . "\n");
}
```

###[file_get_contents](http://php.net/manual/ja/function.file-get-contents.php)
ファイルの内容をすべて取得する

`string file_get_contents( string filename [, bool use_include_path ] )`

```php
// 例
$str = file_get_contents("./hoge.txt");
print($str);
```