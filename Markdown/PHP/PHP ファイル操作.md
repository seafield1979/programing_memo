#ファイル操作 file
ファイルの内容を読み込んだり、書き込んだりする。

##基本操作
###サンプル

// hoge.txt というファイルに "hogehoge" 
という文字列を書き込む

```php
$fp = fopen("hoge.txt", "w");
fputs($fp, "hogehoge");
fclose($fp);
```

###開くfopen
handler fopen( string filename , string mode [, int use_include_path ] )

|パラメータ|説明|
|---|---|
|handler|成功時:ファイルポインタ / 失敗時: FALSE
|filename|オープンするファイルのパス。

mode

|パラメータ|説明|
|---|---|
|'r'| 読み込みのみでオープンします。ファイルポインタをファイルの先頭に置きます。
|'r+'|  読み込み／書き出し用にオープンします。 ファイルポインタをファイルの先頭に置きます。
|'w'| 書き出しのみでオープンします。ファイルポインタをファイルの先頭に置き、 ファイルサイズをゼロにします。ファイルが存在しない場合には、 作成を試みます。
|'w+'|  読み込み／書き出し用でオープンします。 ファイルポインタをファイルの先頭に置き、 ファイルサイズをゼロにします。 ファイルが存在しない場合には、作成を試みます。
|'a'|書き出し用のみでオープンします。ファイルポインタをファイルの終端に置きます。 ファイルが存在しない場合には、作成を試みます。 このモードは、fseek() では何の効果もありません。 書き込みは、常に追記となります。
|'a+'|読み込み／書き出し用でオープンします。 ファイルポインタをファイルの終端に置きます。 ファイルが存在しない場合には、作成を試みます。 このモードは、fseek() では読み込み位置のみに影響します。 書き込みは、常に追記となります。


###閉じる fclose
~~~php
bool fclose( resource handle )

fclose($fp)
~~~

##ファイルから読み込む
hoge.txt から全行を読み込む

```php
if (!($fp = @fopen("hoge.txt", "r"))) {
  exit("failed to open file\n");
}
while( ! feof( $fp ) ){
  echo fgets( $fp, 9182 );
}
```

##ファイルに書き込む
hoge.txt に "hello world"を書き込む

```php
if(!($fp = fopen("hoge.txt", "w+"))) {
  exit("file open error");
}
fputs($fp, "hello world");
fclose($fp);
```

###１行読み込む fgets
string fgets( resource handle [, int length ] )
ファイルポインタから 1 行取得

```php
while( ! feof( $fp ) ){
  echo fgets( $fp, 9182 );
}
```

###１件読み込む fread

```php
//handle が指すファイルポインタから最高 length バイト読み込む
//戻り値:
//ファイルポインタから読み出した文字列／FALSE（ファイルが終端に達したかエラーの場合）

string fread ( resource $handle , int $length )
```

###１件書き込む fwrite fputs
文字列を書き込む fwriteとfputsは同じ機能

```php
/* handle  fopenで開いたファイルハンドル
   戻り値  書き込んだバイト数、またはエラー時に FALSE を返す
 */
fputs ( resource `$handle`, string `$string` [, int 
write_length]);

int fwrite ( resource `$handle`, string `$string` [, int $length ] );
```

例: ファイルの全行を表示

```php
if (!($fp = fopen("./hoge.txt", "r"))) {
  return;
}
while( ! feof( $fp ) ){
  echo fgets( $fp, 9182 );
}
fclose($fp);
```