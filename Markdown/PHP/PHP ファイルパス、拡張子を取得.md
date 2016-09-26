#ファイル情報を取得する fileinfo
// pathinfo を使用する

```php
$file_info = pathinfo('web/public_html/index.php');
 
echo $file_info['dirname']."\n";    // ディレクトリ名
echo $file_info['basename']."\n";    // ファイル名
echo $file_info['extension']."\n";    // 拡張子
echo $file_info['filename'];        // 拡張子を除いたファイル名 ( PHP 5.2.0 以降 )
```