<link href="http://jasonm23.github.com/markdown-css-themes/markdark.css" rel="stylesheet"></link>

phpメモ

[TOC]

#基本 basic

PHPの特徴

  * 文法がCに酷似している
  * 文字列の扱いが簡単。文字列を扱う便利関数がたくさん
  * クラスが使える
  * enumがない
  * ファイル操作が簡単
  * ライブラリが豊富
  * フレームワークも豊富
  * 日々進化している。新しいバージョンではモダンな機能が追加される

###htmlでphpの処理を書く
htmlの中にphpの処理を書く場合は

  * ファイル名を .php にする
  * html内のphp処理を <?php ~ ?> で囲む

とすると、sublimeでhtmlのタグもphpの公文も色分けされるので見やすい。

#セットアップ setup
###Linux
ルート(管理者権限のあるユーザー)で作業する  
~~~
$su -

##PHPをインストール
# yum -y install php php-mbstring php-mysql php-gd
"complete!" と表示されたら成功
~~~

PHPの設定 (/etc/php.iniを編集)

設定ファイル php.ini

~~~ini
/etc/php.ini
  #エラーログの出力パス
  error_log = [パス]  
  ※ error_log = /var/log/php_errors.log のように設定する

  #文字コード変更
  ;mbstring.internal_encoding = EUC-JP ->
  mbstring.internal_encoding = UTF-8

  # 変更前
  ; extension_dir = “ext”
  date.timezone =
  ;extension=php_mbstring.dll
  ;mbstring.language = Japanese
  ;extension=php_mysql.dll

  # 変更後
  extension_dir = "(ドライブ文字):\php\ext"
  date.timezone = "Asia/Tokyo"
  extension=php_mbstring.dll
  mbstring.language = Japanese
  extension=php_mysql.dll

~~~

###Windows
  ダウンロード＆インストール(Windows10 & PHP7)
  phpのzipファイルをダウンロードする。(ファイル名はphp-7.0.9-Win32-VC14-x64.zipとか)
  zipを展開する
  展開したフォルダを c:\php あたりに移動＆リネームする。
  展開したフォルダ内にあるbinフォルダにパスを通す。

  ※コマンドラインからphpを実行して「VCRUNTIME140.dllがない」というエラーが出たら 
    Visual C++ 2015 ランタイム  
    http://www.microsoft.com/ja-jp/download/details.aspx?id=48145  
  をインストールするとよい  


#PHPのファイル files
**Mac**

|ファイルパス|説明|
|!--|!--|
/private/etc/php.ini         | php設定ファイル
/Library/WebServer/Documents | Webドキュメントルート

**Linux**

|ファイルパス|説明|
|!--|!--|
/etc/php.ini             | PHP設定ファイル
/var/log/php/errors.log  | エラーログ<br>ファイルパスはphp.ini の error_log プロパティで設定する
/Applications/MAMP/htdocs  | MAMPのWebドキュメントルート


phpファイルの置き場所  


#対話モード(インタラクティブシェルモード)  interactive 
<!-- interactive:: -->
**Windows**
~~~
php -a 
でインタラクティブモードを起動し、
  echo "hello world";
  と入力しても何も表示されない。

  インタラクティブモードで処理を実行させるには
  <?php
    echo "hello world";
  ?>
  などと <?php ?> で処理を囲ってから Ctrl + Z を入力する必要がある。
~~~
**Mac,Linux**  
-a オプションでphpを起動する
~~~sh
$ php -a 
php > echo "hello world!!";
hello world
php >
~~~
    
#hello world
<!-- hello:   -->
###コンソールから実行
コンソールから実行するには拡張子.php のファイルを作成し
~~~php
- helloworld.php -
<?php
  echo "hello world!!\n";
?>
~~~
~~~sh
$ php helloworld.php
~~~
のようにしてphpコマンドの引数としてphpファイルを実行する

###HTMLファイル内のphpプログラムを実行
htmlファイル内でPHPプログラムを動作させてみる
~~~html
- php_test.html -

<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <!-- <?php ~ ?> 内にPHPプログラムプログラムを記述する -->
 <?php
    echo '<p>Hello World</p>';
  ?>
 </body>
</html>
~~~

###PHPのログを出力する

**エラーログ出力 error_log**  
~~~php
//　エラーログ出力
error_log("hogehoge");

// 任意のファイルにし出力 (第2引数に3を設定)
error_log("hoge","3","./error.log");

// 標準出力をしないprint_rを使う
error_log(print_r($array1, true));
~~~

<!-- log:: logs::  -->
**php.iniの設定**  
html上で実行されたphpプログラムのエラーログを出力するには php.ini で設定を行う必要がある。

~~~ini
;htmlにエラーを出力するかどうか
display_errors = On/Off
  
;ログの出力レベル
error_reporting = E_ALL | E_STRICT

;ファイルに出力
log_errors = On/Off

;エラーログのパス
error_log = /var/log/php/error.log
~~~
上記設定を行ってもエラーログがログファイルに出力されない場合は、エラーログファイルの出力ディレクトリ、出力ファイルに書き込み権限が設定されているか確認してみる。ログファイルのディレクトリを丸ごと chmod 777 で全ユーザに書き込み設定しておくのもあり。


**ログの出力(tailを使用して常に監視)**  
`$tail -50f /var/log/php/error.log`

**php.iniの変更を反映させるためにはApacheをリスタートする**  
`$sudo apachectl restart`

**自前のログ出力メソッドを使う**
~~~php
<?php
  
?>
~~~


#文字列表示 print
<!-- print:: print_r:: var_dump::-->
~~~php
<?php

//echo
  $str = "hello world!!";
  echo "hogehoge" . $str1 . "\n";

//print_r
  見やすい形に整形して文字列出力する
  $array1 = array("1","2");
  print_r($array);
  // 出力
    Array
    (
        [0] => 1
        [1] => 2
    )

  // 第２引数にtrueを指定することで戻り値として返す
  $str = print_r($array, true);

//var_dump
  // 標準出力に表示
  var_dump($array1);

  // 変数に出力(はそのままではできないのでバッファを使用する)
  // $array の内容var_dumpしたものを $str に格納する
  ob_start();
  var_dump($array1);
  $str = ob_get_contents();
  ob_end_clean();
~~~

#基本 basic
<!-- basic:: -->
**コメントアウト**
~~~php
<?php
  // 一行コメントアウト
  #  一行コメントアウト
  /* 範囲コメントアウト */
?>
~~~

__プログラムを終了__:  
~~~
// プログラム終了直前に文字列を表示する  
void exit([string $status])
    
//呼び出し元に終了ステータスを返す
void exit(int $status)
~~~

#変数 variable
<!-- var:: variable:: -->
PHPでは未定義の変数を参照してもエラーにならずnullが返る。

__宣言__:  

    $変数名 = 初期値;    // 初期値で初期化
    $変数名 ＝ NULL;    // NULL初期化
__代入__:

    $変数 = 値;
    $変数1 = $変数2;  // 変数1に変数2が入る

__グローバル変数__:  
関数内でグローバル変数が使えない。
関数内でグローバル変数を使う場合は global をつける

    例: プログラムの引数 $argv を使用する
    function hoge{
      global $argv;
      print_r($argv);
    }

###変数の型  
|型|説明|
|!--|!--|
  **boolean**| true / false をとる。大文字、小文字は区別しない  
  **整数**| 整数値 32bit環境なら -2147483648 ~ 2147483647  
  **浮動小数点**| 1.2345 みたいな少数の値  
  **文字列**| "hoge" のような文字列 String
  **配列**| 複数の要素を持つ。PHPの配列は全てキーをもとに要素の値を取得する辞書型。
  _オブジェクト_|  
  _NULL_|  

###型変換・キャスト
<!-- cast:: -->
~~~
//文字列 -> 整数値
$int1 = (int)"123";  // キャスト
$int2 = intval("123");   // 整数値に変換
print(gettype($int1));   // integer;

//integer -> string
$str = (string)100;
print(gettype($str));   // string 

//float -> integer
$int1 = (int)1.2345;
$int2 = intval(1.2345);
print(gettype($int1) . " " . gettype($int2));  // integer integer
~~~

* (int), (integer) - 整数へのキャスト
* (bool), (boolean) - 論理値へのキャスト
* (float), (double), (real) - float へのキャスト
* (string) - 文字列へのキャスト
* (array) - 配列へのキャスト
* (object) - オブジェクトへのキャスト
* (unset) - NULL へのキャスト (PHP 5)

###変数の型判定　gettype
<!-- gettype:: -->
変数の型を判定する方法 
gettypeメソッドは変数の型を返す。
~~~
  echo gettype("hoge");   // string 

  返された文字列は、以下のいずれかの値を持ちます。
  "boolean"
  "integer"
  "double" (歴史的な理由により、float の場合には "double"が返されます。"float" とはなりません)
  "string"
  "array"
  "object"
  "resource"
  "NULL"
  "unknown type"
~~~

###各種メソッドを使って判定する
変数の型やnullかどうかなどのチェックを行う便利関数が用意されている。
|メソッド名|説明|
|---|---|
  get_class() | オブジェクトのクラス名を返す
  is_array() | 変数が配列かどうかを検査する
  is_bool() | 変数が boolean であるかを調べる
  is_callable() | 引数が、関数としてコール可能な構造であるかどうかを調べる
  is_float() | 変数の型が float かどうか調べる
  is_int() | 変数が整数型かどうかを検査する
  is_null() | 変数が NULL かどうか調べる
  is_numeric() | 変数が数字または数値形式の文字列であるかを調べる
  is_object() | 変数がオブジェクトかどうかを検査する
  is_resource() | 変数がリソースかどうかを調べる
  is_scalar() | 変数がスカラかどうかを調べる
  is_string() | 変数の型が文字列かどうかを調べる
  function_exists() | 指定した関数が定義されている場合に TRUE を返す
  method_exists() | クラスメソッドが存在するかどうかを確認する

## 異なる型の加算
数値と文字列の加算(+)は、文字列が数値として評価できた場合にのみ数値として加算される。

    $foo = 1 + "10.5";              // $foo は float です (11.5)
    $foo = 1 + "-1.3e3";            // $foo は float です (-1299)
    $foo = 1 + "bob-1.3e3";         // $foo は integer です (1)  
    $foo = 1 + "bob3";              // $foo は integer です (1)  後の文字列は無視される


#インクルード include
<!-- include:: require:: require_once:: -->

    外部ファイルを読み込む。ファイルパスを指定しない場合は php.ini の include_path で指定したパスを検索する。
    includeは致命的なエラーを発生させないが、requireは致命的なエラーが発生してスクリプトが停止する可能性がある。
      include 'hoge.php';   //パスを指定しない php.ini の include_path を探す
      include './hoge.php'; //パスを指定

    使い分け
      includeとrequireの使い分け
        http://furudate.hatenablog.com/entry/2013/10/17/141557
      include  ページの内容を差し込みたい
      require  PHPの処理を読み込みたい。同じ処理を何度も読み込む必要はないので、通常は require_once を使う。

require で呼び出したphpファイルに標準の引数を渡す方法(requireするphpはメソッドやクラスの定義だけなので普通はやらない！)
~~~php
// hoge.php に引数を渡す
$argv = array("1 2 3", "1", "2", "3");
$argc = cont($argv);
require_once("hoge.php");

--- hoge.php ---
// 引数を表示する
foreach($argv as $arg) {
  print($arg . "\n");
}
~~~

#定数 const
<!-- const:: define:: -->
~~~php
<?php
//define(識別子, 値);
define("TAX", 0.08);
define(TAX2, 0.10);   // ダブルコーテーションなしでもOK
?>
~~~

## 自動的に定義される定数

|定数名|説明|
|!--|!--|
  `__LINE__` | ファイル上の現在の行番号。
  `__FILE__` | ファイルのフルパスとファイル名 (シンボリックリンクを解決した後のもの)。 インクルードされるファイルの中で使用された場合、インクルードされるファイルの名前が返されます。
  `__DIR__` | そのファイルの存在するディレクトリ。include の中で使用すると、 インクルードされるファイルの存在するディレクトリを返します。 つまり、これは dirname(`__FILE__`) と同じ意味です。 ルートディレクトリである場合を除き、ディレクトリ名の末尾にスラッシュはつきません。
  `__FUNCTION__` | 関数名。
  `__CLASS__` | クラス名。 クラス名には、そのクラスが宣言されている名前空間も含みます (例 Foo\Bar)。 PHP 5.4 以降では、__CLASS__ がトレイトでも使えることに注意しましょう。トレイトのメソッド内で __CLASS__ を使うと、そのトレイトを use しているクラスの名前を返します。
  `__METHOD__` | クラスのメソッド名。
  `__NAMESPACE__` | 現在の名前空間の名前。

#文字列 string
<!-- string:: -->
文字列の連結は`'.'`  
	`"hoge" . "hoge" . $str1`

~~~php
<?php
シングルコート文字列 

//シングルコード文字列はエスケープ(\nや\t)や変数は展開されない
// '文字列'
echo 'hello\n'       // hello\n  と表示される
echo 'hello $hoge'   // hello $hoge と表示される

// ダブルコート文字列
// ダブルコートで囲んだ文字列内のエスケープや変数は展開される
// "文字列"
echo "hello\n";     // hello の後に改行される
echo "hello$hoge";  // $hogeが展開されて表示される
echo "hello${hoge}"   // $hogeが展開されて表示される。上と同じ

//文字列の要素にアクセス
ある文字列を１文字づつ取り出して処理を行う方法

$hoge = "hoge";
echo $hoge[0];    // h
foreach (str_split($hoge) as $value){
  echo $value . "\n";   // h,o,g,e が出力される
});

//文字列の長さ strlen:
strlen(文字列)
※文字列以外の値を渡すとエラーになる。is_stringでチェックが必要

//文字列に変換 strval:
$変数 = strval(123);    // 変数には返還後の文字列"123"が入る

// 文字列の検索(見つかった位置を返す)
// 検索文字の見つかった位置 = strpos(文字列, 検索文字列)
$pos = strpos("hoge123", "123");

// 文字列の切り取り
// string substr(string string, int start [, int length])
// 切り取った文字列 substr("分割対象の文字列", 開始位置 [, 切り取る長さ])
$str2 = substr("hogehoge123", 4);   // $str2 = "hoge123"
~~~

###パターンマッチ preg_match
<!-- preg_match:: -->
  preg_match(パターン, 対象文字列, 結果が格納される変数);
  戻り値: マッチしたら1 マッチしなかったら0

    preg_match('/(.+)\.js/', $src_string, $m);
    $m[0] -> ヒットした文字列全体
    $m[1] -> １つ目の()にヒットした文字列
    $m[2] -> ２つ目の()にヒットした文字列

###エスケープ文字
  ダブルコート内で使用出来るエスケープ。エスケープを使用すると改行やタブ、特殊文字を出力できたりする。

    \n 改行
    \t tab
    \$ $
    \\ \
    \"  "

###ヒアドキュメント 
  複数行に渡る文字列をそのままの形で書くことができる。
<<<[適当な文字列]  
設定したい文字列  
[適当な文字列];  

    例:
    echo <<<hoge
      aaa
      bbb
      ccc
    hoge;

#演算子 operator
<!-- operator:: -->
###四則演算
    +,-,*,/
    ++    インクリメント。１増やす
    --    デクリメント。１減らす

###比較演算子
if文やwhile文の条件式に使用する

  |演算子|説明|
  |!--|!--|  
  == |   型を相互変換した上で値が同じか
  != |  型を相互変換した後で値が同じでないか
  === |  値と型が同じか
  !== |  値と型のどちらかが同じでないか

~~~php
if ($val == 100) {
}
~~~

###論理演算子
  |演算子|説明|
  |!--|!--|  
  ! |    not  if(!true) で falseになる 
  && |   条件1 && 条件2  で両方がtrueでtrue
  &#124;&#124; |   条件1 &#124;&#124; 条件2  でどちらかがtrueでtrue

#配列 array
  PHPの配列はすべて連想配列。2次元配列でも子要素の要素数はバラバラにできる

**初期化**
~~~php
<?php
// 空の配列
$配列名 = array();

// キー名 & 値
$配列名 = array(key1=>value1, key2=>value2, ...);

例:  $array1 = array(a=>1, b=>2, c=>3);
    $array2 = array("a"=>1, "b"=>2, "c"=>3);

// キー名を省略
$配列名 = array(value1, value2, ...);
例:  $array1 = array("hoge", "hoge2", "hoge3");
~~~

**参照**
~~~php
<?php
//配列の要素を参照する場合は
$配列名["キー"]; 

//キーなしで配列を宣言した初期化した場合は 0 から要素が入っていく
$array = array(1,2,3,4,5);    // [0]=>1, [1]=>2, [2]=>3, [3]=>4, [4]=>5

$array = array(1,2,3,4,5);
echo $array[0];   // 1
echo $array[2];   // 3
~~~

**配列のあれこれ**

~~~php
<?php
//要素数を取得
count($配列);

//配列の中に配列(多次元配列)
//PHPの多次元配列は要素ごとに配列のサイズが異なる
$array1 = array("hoge", array("hoge21", "hoge22", array("hoge31", "hoge32")));
//配列を表示:
var_dump($配列);
print_r($配列);

//要素の追加:
// キーを指定する方法
$arr[キー] = 値;

// 配列の最後に追加
$arr[] = 値;

//要素の削除: unset
$array = array(a=>1, b=>2, c=>3);
unset($array[b]);

//要素が存在するかどうかを判定する
$ret = array_key_exists($配列[キー]);

$array=(a=>1,b=>2,c=>3);
array_key_exists($array[a]);    // true
array_key_exists($array[e]);    // false

//全要素のアクセス
$array = array(a=>1, b=>2, c=>3, d=>4);

// 値
foreach($array as $value) {
    echo "value=$value\n";
}
// キーと値
foreach($array as $key=>$value) {
    echo "key=$key value=$value\n";
}
~~~

#if文
<!-- if:: -->
~~~php
<?php
// if
if(条件式){
  処理
}

// if ~ else
if(条件式){
  真のときの処理
}
ele {
  偽のときの処理
}

// if ~ elseif ~ else
if(条件式){
  処理
} elseif(条件式) {
  処理 
} else {
  処理
} 

条件には以下のような常にtrue,falseになる書き方ができる
if (true) or (1)     // 常にtrue
if (false) or (0)    // 常にfalse
~~~

#loop
###for
<!-- for:: -->
~~~php
for ($cnt=0; $cnt < 10; $cnt++) {
    echo "cnt=${cnt}\n";
}
~~~

###foreach
<!-- foreach:: -->
~~~php
<?php
// 基本
$array = array(name=>"shutaro", age=>36, comment=>"hoge");
foreach ($array as $key=>$value) {
  echo "key=${key} value=${value}\n";
}

// ループの最初と最後を取得
$array = array(name=>"shutaro", age=>36, comment=>"hoge");
foreach ($array as $key=>$value) {
  if ($value == reset($array)) {
    // 最初
  }
  else if ($value == end($array)) {
    // 最後
  }
  echo "key=${key} value=${value}\n";
}
?>
~~~

###while
<!-- while:: -->
~~~php
<?php
//while(条件式){
//  処理
//}
//条件式がtrueの間はずっと処理を行う。

$cnt = 10;
while($cnt > 0){
  echo "cnt=${cnt}\n";
  $cnt--;
}
~~~

#switch文
<!-- switch:: -->
Cとほぼ一緒だが、caseの値に文字列が使用出来る。が、文字列の比較に==演算子を使っているので100と"100"が同じとみなされるため注意。

~~~php
<?php
switch ($i) {
case 0:
    echo "iは0に等しい";
    break;
case 1:
    echo "iは1に等しい";
    break;
case 2:
    echo "iは2に等しい";
    break;
default:
   echo "iは0,1,2に等しくない";
}

// caseに文字列が使える
switch ($str) {
case "hoge":
    echo "str は hoge";
    break;
case "mage":
    echo "str は mage";
    break;
default:
   echo "hogeでもmageでもない";
}

?>
~~~

#関数 function
 <!-- func: function: -->

+ PHPでは関数呼び出しの後に関数の宣言を定義してもOK
+ PHPでは関数がグローバルスコープにある
+ 再起呼び出しが可能

###基本  
~~~php
<?php
// 関数の宣言
fucntion 関数名($引数1, $引数2, ...){
    // 処理
    return 戻り値;
}
function hogeFunc($arg1, $arg2) {
    return $arg1 . $arg2;
}

//呼び出し
$戻り値 = 関数名(引数1, 引数2, ...);  

// 先頭に@をつけると関数内で発生したエラーが出力されなくなる
$戻り値 = @関数名(引数1, 引数2, ...);   

// グローバル変数を関数内で参照するには関数内で global 定義する
$globalStr = "hoge";
func hogeFunc() {
  global $globalStr;

  echo $globalStr;
}
~~~

###動的な関数の有効化
関数の中に関数定義を行うことで、外の関数が呼び出された後に中の関数が利用可能になる。
~~~php
<?php
function func1(){
    function func2(){
        echo "func2";
    }
}
// func2();  未定義エラー

func1();        // func1のfunc2が有効になる
func2();        // ok
?>
~~~

###引数の参照渡し
関数の引数を参照型にすることで、関数内で引数に渡した変数や配列の内容を変更することができる
~~~php
<?php
// 参照渡しにしたい変数の先頭に & をつける
function hoge(&count) {
  $count++;
}
$count1 = 10;
hoge($count1);
echo $count1;   // hogeメソッドの中でインクリメントされたので 11 になる
?>
~~~

#クラス class
<!-- class:: object:: -->
PHPのクラスは以下のような機能を持つ。
※オブジェクトはクラスから生成された実態。インスタンスともいう。
  
###基本
~~~php
<?php
//クラスの定義:
class Hoge {
  // クラス外から参照できるプロパティ
  public $publicStr = "公開";
  public $name = "";

  // クラス外から参照できないプロパティ
  private $privateStr = "極秘";

  // コンストラクタ
  function __construct($name) {
    $this->name = $name;
  }
  // デストラクタ (参照元が１つもなくなった時に呼ばれる)
  function __destruct() {      
  }

  public function hogeFunc(){
    // 処理
  }
}

##インスタンス(オブジェクト)生成
// $instance = new クラス名();
$hoge1 = new Hoge("hohoho");

  もしくは

// $instance = new クラス名;
$hoge2 = new Hoge;

//メンバ参照
// $instance->プロパティ変数;
$hoge->name;

//メソッド呼び出し
$instance->メソッド();

//インスタンスの参照:  
//生成済みのインスタンスを別の変数に代入すると、代入先の変数は代入元の変数の参照になる。
//参照なので実態は１つだけ。よって参照元、参照先の変更はどちらにも反映される。

$hoge = new Hoge();
$hoge2 = $hoge;   // $hoge と $hoge2は同じ
~~~

###オブジェクトのクローン作成
<!-- clone: -->
PHPではインスタンス変数の代入は参照を作るだけなので、別のインスタンスを作りたい場合は clone を使用する

~~~php
<?php
$hoge = new CHoge();
$hoge2 = clone $hoge;
$hoge->name = "shutaro";
$hoge2->name = "naotaro";
// これで $hoge->name と $hoge2->name の値はそれぞれ別の値になる。

//クローン時に実行されるメソッドを定義できる

class Hoge{
  public $id = 0;
  function __clone(){
    ~クローン時の処理~
    個別のインスタンスで別々の値を持ちたいときなどに、ここで新たに確保する
  }
}
?>
~~~

##アクセス制限
<!-- public:: private:: protected:: -->
クラスのプロパティやメソッドの先頭にアクセス制限を指定するパラメータを指定できる。

|アクセス制限子|説明|
|---|---|
  public | クラスの外から参照可能  
  private | クラスの外から参照不可  
  protected | 定義したクラスと継承したクラスからのみ参照可能  

###static
<!-- static:: -->
staticをつけたプロパティやメソッドはクラスのインスタンスなしに呼び出すことができる。C++で言うところのclass変数、classメソッド。

~~~php  
Class CTest1 {
  public static $name = "SHUTARO";
  public static function static_func(){
    echo self::$name . "\n";
  }
  // staticプロパティの参照
  echo CTest1::$name . "\n";
  // staticメソッドを呼び出す
  CTest1::static_func();
}
~~~

###finalキーワード
<!-- final:: -->
final メソッドは継承先のクラスで上書きできない。  
final クラスは継承できない。  

~~~php
class Hoge {
  final public function hoge(){
    // 子クラスで上書き不可
  }
}

final class CHoge{
  // 拡張（継承）不可  
}
~~~

###オブジェクト定数
<!-- class constant:: -->
変更できない定数。インスタンスなしでアクセス可能なのでstatic変数に似ている。

~~~php  
class MyClass
{
    const CONSTANT = 'constant value';

    function showConstant() {
        echo  self::CONSTANT . "\n";
    }
}
echo MyClass::CONSTANT . "\n";
~~~

##コンストラクタとデストラクタ construct destruct
<!-- construct: destruct: -->
コンストラクタはオブジェクトが生成される時、デストラクタはオブジェクトが破棄される時に呼ばれるメソッド。コンストラクタではプロパティの初期化やリソースの読み込み等を行い、デストラクタではリソースの解放等を行う。

~~~php
<?php
class CTest1
{
    public $name = null;
    // コンストラクタ
    function __construct($name) {
        $this->name = $name;
    }
    // デストラクタ (参照元が１つもなくなった時に呼ばれる)
    function __destruct() {
        echo "destroy $this->name \n";
    }
    public function disp(){
        echo $this->name . "\n";
    }
}
// コンストラクタが呼ばれる
$hoge = new CTest1('shutaro');
$hoge->disp();
$hoge = null;       // ここで$hogeのインスタンスへの参照がなくなるのでデスクトラクタが呼ばれる
~~~

##継承 extends
<!-- extends:: -->
クラスの継承を行うことで継承元のクラス(親クラス)の機能をそのまま引き継いだ子クラスを定義することができる。  
PHPでは継承先のクラスは継承元の public,protectedのプロパティ、メソッドを引き継ぐ。privateは引き継がない。
  子クラスから親クラスのメソッドを呼ぶには parent::メソッド名() を使用する。

~~~php
<?php
class CTest1{
  function __construct(){
    echo "CTest1::__construct\n";
  }
  public function disp(){
    echo "CTest1::disp\n";
  }
}

class CTest2 extends CTest1 {
  function __construct(){
    parent::__construct();
  }
  public function disp(){
    parent::disp();
  }
}
?>
~~~
<!-- ■標準クラス standard:: -->

#例外処理 exception
<!-- exception:: -->
PHPもC＋＋と同じ感じで例外をキャッチできる。何かしらのエラーが起きた時に、そのエラーを受け取るブロックまで自動で処理が遷移してエラー処理を行うことができる。

~~~php
<?php
try {
  // 例外が発生するかもしれない処理を try スコープに入れる
  throw new Exception("error1");
} catch (Exception $e){
  echo $e->getMessage . "\n";
} finnaly {
  // 必ず実行される処理
} 
~~~

Exceptionを継承した自前の例外クラスを使ったエラーハンドリング処理

~~~php
<?php  
class MyException extends Exception { }
class MyException2 extends Exception { }

class Test {
    public function testing() {
        try {
            try {
                // ここでエラーを投げる
                throw new MyException('foo!');
                throw new MyException2('woo!');
            } catch (MyException $e) {
                // MyException の throwはこちらでキャッチされる
                // 改めてスロー
                throw $e;
            } catch (MyException2 $e) {
                // MyException2 の throwはこちらでキャッチされる
                throw $e;
            }
        } catch (Exception $e) {
            var_dump($e->getMessage());
        } finally {
            echo "finnaly!\n";
        }
    }
}

$foo = new Test;
$foo->testing();
~~~
  
#名前空間 namespace
<!-- namespace:: -->
[PHPの名前空間について簡単にまとめてみた](http://ucwd.jp/blog/736)
  独自の名前空間を使用しない場合、宣言したメソッドやクラス、グローバル変数はすべて同じ名前空間に入るため、名前がかぶってエラーになってしまう。名前がかぶらないようにするためには、必要に応じてファイル単位で別々の名前空間を宣言する必要がある。
  
~~~php
<?php
- namespace1.php -
namespace NS1;

// 以下のメソッドは名前空間 NS1 に属する
function hoge(){
  echo "hoge1\n";
}

- namespace2.php -
namespace NS2;

// 以下のメソッドは名前空間 NS2 に属する
// namespace1.php にある関数 hoge と同じ名前
function hoge(){
  echo "hoge2\n";
}

- test.php -

require_once "./namespace1.php";
require_once "./namespace2.php";


//hoge();   エラー Call to undefined function hoge()
NS1/hoge();
NS2/hoge();
~~~

#エイリアスとインポート alias
<!-- alias:: -->
  名前空間を指定してメソッドを呼び出す場合に  
    `\My\Space\Is\Very\Small\echo_message();`  
  のように長い名前空間をつけるのは煩雑になるので、エイリアスとインポートを使って記述を短縮することができる。

##エイリアス: aslias
    名前空間名を別の名前でも呼び出せるようにする
    -- use 元の名前空間 as 別名の名前空間
    require_once 'file02.php';
    use My\Space\hoge\hage\Anpontan as MySpace;

    MySpace\echo_message();
    $obj = new MySpace\MySpaceClass();

##インポート: import
  `use 元の名前空間 + その名前空間に含まれるクラス名 as 別名のクラス名`  
  名前空間 My\Space に属する MySpaceClass クラスに直接エイリアスを定義し、且つその別名で My\Space\MySpaceClass を呼び出せる

    require_once '20130806_02.php';

    use My\Space\MySpaceClass as MyClass;
    $obj01 = new MyClass();


#ファイル操作 file
<!-- file:: -->
ファイルの内容を読み込んだり、書き込んだりする。

##基本操作
###サンプル
~~~php
<?php
// hoge.txt というファイルに "hogehoge" という文字列を書き込む
$fp = fopen("hoge.txt", "w");
fputs($fp, "hogehoge");
fclose($fp);


?>
~~~

###開くfopen
<!-- fopen:: -->

    handler fopen( string filename , string mode [, int use_include_path ] )

    handler
      成功時:ファイルポインタ / 失敗時: FALSE
    filename
      オープンするファイルのパス。
    mode
      'r' 読み込みのみでオープンします。ファイルポインタをファイルの先頭に置きます。
      'r+'  読み込み／書き出し用にオープンします。 ファイルポインタをファイルの先頭に置きます。
      'w' 書き出しのみでオープンします。ファイルポインタをファイルの先頭に置き、 ファイルサイズをゼロにします。ファイルが存在しない場合には、 作成を試みます。
      'w+'  読み込み／書き出し用でオープンします。 ファイルポインタをファイルの先頭に置き、 ファイルサイズをゼロにします。 ファイルが存在しない場合には、作成を試みます。
      'a' 書き出し用のみでオープンします。ファイルポインタをファイルの終端に置きます。 ファイルが存在しない場合には、作成を試みます。 このモードは、fseek() では何の効果もありません。 書き込みは、常に追記となります。
      'a+'  読み込み／書き出し用でオープンします。 ファイルポインタをファイルの終端に置きます。 ファイルが存在しない場合には、作成を試みます。 このモードは、fseek() では読み込み位置のみに影響します。 書き込みは、常に追記となります。
###閉じる fclose
<!-- fclose:: -->
    bool fclose( resource handle )
###１行読み込む fgets
<!-- fgets:: -->
    string fgets( resource handle [, int length ] )
    ファイルポインタから 1 行取得

    while( ! feof( $fp ) ){
      echo fgets( $fp, 9182 );
    }

###１件読み込む fread
<!-- fread:: -->
    string fread ( resource $handle , int $length )
    handle が指すファイルポインタから最高 length バイト読み込む

    戻り値:
    ファイルポインタから読み出した文字列／FALSE（ファイルが終端に達したかエラーの場合）

###１件書き込む fwrite fputs
<!-- fwrite:: fputs:: -->
    文字列を書き込む fwriteとfputsは同じ機能
    fputs ( resource $handle, string $string [, int write_length]);
    int fwrite ( resource $handle , string $string [, int $length ] );
    ファイルポインタに文字列を書き込む。

    戻り値
    書き込んだバイト数、またはエラー時に FALSE を返します。
  例: ファイルの全行を表示
    
    if (!($fp = fopen("./hoge.txt", "r"))) {
      return;
    }
    while( ! feof( $fp ) ){
      echo fgets( $fp, 9182 );
    }
    fclose($fp);
  
##ファイル操作関数

###カレントディレクトリを変更 chdir
<!-- chdir: -->
bool chdir ( string $directory )  
PHP のカレントディレクトリを directory に変更します。


###ファイル名を変更 rename
<!-- rename: -->
bool rename ( string $oldname , string $newname [, resource $context ] )

###親フォルダ名を取得 dirname
<!-- dirname -->
string dirname ( string $path [, int $levels = 1 ] )

###全行を配列に読み込む:  file()
    array file( string filename [, int use_include_path ] )
    
    戻り値:
    ファイルのデータが格納された配列／FALSE（失敗した場合）

###ファイルの内容を読み込む 
<!-- file_get_contents: -->
    ファイルの内容をすべて取得する
    string file_get_contents( string filename [, bool use_include_path ] )

    例:
    $str = file_get_contents("./hoge.txt");

##ファイルから読み込む
    // hoge.txt から全行を読み込む
    if (!($fp = @fopen("hoge.txt", "r"))) {
      exit("failed to open file\n");
    }
    while( ! feof( $fp ) ){
      echo fgets( $fp, 9182 );
    }

##ファイルに書き込む
    // hoge.txt に "hello world"を書き込む
    if(!($fp = fopen("hoge.txt", "w+"))) {
      exit("file open error");
    }
    fputs($fp, "hello world");
    fclose($fp);

#データベース操作  database
<!-- mysql:: db:: database:: -->
既にphpとMySQLが使用できる状態にしてある前提で進める
phpで以下のメソッドを使用する

##MySQLを使用する MySQL
  phpにはMySQL接続のためのAPIが３種類用意されてある。  
  mysql と mysqli そして PDO  
  mysqlは古いので使用しない方がよいらしい。  
  [どの API を使うか](http://php.net/manual/ja/mysqlinfo.api.choosing.php)
  ここではmysqli を使ってMySQLのテーブルを操作してみる。

###接続〜切断の流れ
~~~php
<?php
  //データベースに接続
$mysqli = new mysqli("localhost", "root", "poco", "testdb");
if ($mysqli->connect_error) {
    die('Db Connect Error: '.$dbh->connect_errno.':'.$dbh->connect_error);
}
//文字コードをセット(UTF-8)
$mysqli->set_charset('utf8');

//クエリを発行
// query は
//SELECT, SHOW, DESCRIBE や EXPLAIN 文、その他結果セットを返す文では、 mysql_query() は成功した場合に resource を返します。エラー時には FALSE を返します。
//それ以外の SQL 文 INSERT, UPDATE, DELETE, DROP などでは、 mysql_query() は成功した場合に TRUE 、エラー時に FALSE を返します。
if($result = $mysqli->query("SELECT * FROM table1")){
  if ($result->num_rows > 0){
    //1件づつ取得する
    while($row = $result->fetch_assoc()){
      printf( "%d %s %d %d<br>", $row["id"], $row["column1"], $row["column2"], $row["column3"]);
    }
  }
  // 結果セットを閉じる
  $result->close();
}
// DB接続を閉じる
$mysqli->close();
~~~

###select文
~~~php
<?php
//select文の結果はオブジェクトに返る
if($result = $mysqli->query("SELECT * FROM table1")){
  // 取得したレコードの件数は $result->num_rows に入る
  echo "rows=$result->num_rows";
  // レコードを取得するには $result->fetch_assoc() で１件づつとり出す
  while ($row = $result->fetch_assoc()) {
    // $row["カラム名"] でレコードのカラムを参照できる。
    echo "id=$result->row['name']"
  }
}
else {
  echo "失敗";
}
?>
~~~

###insert文
~~~php
<?php
  INSERT, UPDATE, DELETE, DROP などでは、 mysql_query() は成功した場合に TRUE 、エラー時に FALSE を返します
  if($result = $mysqli->query("INSERT INTO users (name) VALUES ('hoge')")) {
    if ($result == 0) {
      echo "失敗";
    }
    else {
      echo "成功";
    }
  }
?>
~~~


#セッション session
<!-- session:: -->
セッションは一連の接続を管理する仕組み。httpはステートレスなので、通常は連続でページを読み込んでも以前の状態は保持されない。そこで、クライアント側のcookieにセッションIDを保存しておき、サーバー側でこのセッションIDを参照することでユーザーの状態を把握する。
PHPの提供するセッションの仕組みは  
session_start()　でセッション開始、セッション再開
セッションが始まると $_COOKIE["PHPSESSID"] にセッションIDが入る
$_SESSION にセッション変数の保持、ただしセッションが終わると参照できなくなる。
session_destroy() でセッションを終了。

~~~php
<?php

//セッションを開始する:
//htmlファイルの先頭に記述する。おまじないのようなもの
//新しいセッションを開始、あるいは既存のセッションを再開する
//これでCookieに "PHPSESSID" というセッションIDが入ったデータが作成される。
session_start();  

//セッションIDの参照:
//687301552c9e5bd0393ccbf691173d7d  のようなランダムな文字列
$_COOKIE["PHPSESSID"];  
  
//セッション変数の登録:
//そのセッションでだけ有効な変数
//$_SESSION[キー] = 値
$_SESSION["hoge"] = 12345;

//セッッションを終了する:
session_destroy();

###escape
<!-- escape:: -->
htmlで入力された文字をエスケープ

~~~php
<?php
//HTMLエスケープ
htmlspecialchars('you & I > he & she');
// 結果: you &amp; I &gt; he &amp; she

//HTMLアンエスケープ
htmlspecialchars_decode('you &amp; I &gt; he &amp; she');
// 結果: you & I > he & she
~~~

#Cookie
<!-- cookie:: -->
Webページを表示した場合に、ブラウザにCookieを保存しておき次回以降ページを表示した時にそのCookieの値を参照することができる。  
※ChromeではローカルファイルでCookieの保存ができない。

##Cookie基本操作
**Cookieを保存**
~~~php
setcookie( cookieName , value , [timeout] , [path] , [domain] )
setcookie( "hoge", 123, time() + (3*(24*60*60)));
~~~

|引数名|説明|
|---|---|
cookieName | Cookie名
value |      Cookie値
timeout |  ※省略可能。Cookieの有効期限。このCookieが有効となる期限を秒単位で指定する<br>例）0 … ブラウザが閉じられた時点で削除される<br>例）time() + (30 * (24*60*60)) … 30日間有効になります。<br>time()で、現在時刻が『1970年1月1日 00:00:00 GMT』からの通算秒として返されます。<br>そこに30日×(24時間×60時間×60秒)を加算して算出しています。
path | ※省略可能。Cookieが有効なパス。<br>例）"/"
domain |  ※省略可能<br>Cookieが有効なドメイン。<br>例）"www.24w.jp"

**Cookieを取得**
~~~php
var value = $_COOKIE[ cookieName ]
$hoge = $_COOKIE["hoge"];
~~~

**Cookieを削除**
~~~php
setcookie( "クッキー名", "", time()-10000);
// timeに過去の日付を設定すると削除される。
~~~


#小技＆便利  skills
<!-- skills:: -->

###リダイレクト　redirect
<!-- redirect:: -->
  header('location: [パス]');
  ※html本体より前に送信しないといけない。よってheaderの前に echo 文で文字列を出力していたりすると失敗する

###日時時刻取得 date
<!-- date:: -->
```php
<?php
date("Y-m-d H:i:s");

string date ( string $format [, int $timestamp = time() ] )

date_default_timezone_set('Asia/Tokyo');    // タイムゾーンを設定しないとグリニッジ標準時間になる
$date_str = date("Y-m-d H:i:s");
echo $date_str . "\n";

$date_str = date("Y-m-d H:i:d");
$next_day = date("Y-m-d", strtotime("+1 day"));
?>
```
|フォーマット|説明|値|
|---|---|---|
format
d | 日。二桁の数字（先頭にゼロがつく場合も） | 01 から 31
D | 曜日。3文字のテキスト形式。 | Mon から Sun
l | (小文字の 'L') | 曜日。フルスペル形式。 Sunday から Saturday
w | 曜日。数値。 | 0 (日曜)から 6 (土曜)
月 | --- |  ---
F | 月。フルスペルの文字。 |  January から December
m | 月。数字。先頭にゼロをつける。 |  01 から 12
M | 月。3 文字形式。 |  Jan から Dec
t | 指定した月の日数。 |  28 から 31
年 | --- |  ---
Y | 年。4 桁の数字。 |  例: 1999または2003
y | 年。2 桁の数字。 |  例: 99 または 03
時 | --- |  ---
a | 午前または午後（小文字） | am または pm
A | 午前または午後（大文字） | AM または PM
h | 時。数字。12 時間単位。  | 01 から 12
H | 時。数字。24 時間単位。  | 00 から 23
i | 分。先頭にゼロをつける。 | 00 から 59
s | 秒。先頭にゼロをつける。

    

###置換 replace
  	置換後の文字列 = str_replace(置換元, 置換先, 対象文字列)
  	str_replace('-', '/', $aaa); 		'-' -> '/'に変換

    置換元(文字列か文字列の配列)
    置換先(文字列か文字列の配列)

    置換元と置換先が両方とも文字列の場合  ただの置換、置換後の文字列を返す
    置換元が配列で置換先が文字列の場合  置換元の配列全部に対して置換先の文字列で置換、配列を返す
    置換元と置換先が両方とも配列の場合  それぞれの配列から順に文字列を取り出し置換を行う。配列を返す


###POST GET
<!-- post:: get:: -->
  htmlのフォームから送信されたGET/POSTメッセージを取得する  
  送信側

    <form action="test/php" method="post">
  		<input type="submit" placeholder="どう思う？">
  		<input type="hidden" name = "hidden1" value="１２３４">
  		<div class="btn-large-area">
  			<a id="button03" class="btn-def">button3</a>
  		</div>
  	</form>	
	
  受信側

    $_GET['param1']
    $_POST['hidden1']		// "１２３４"


###ディレクトリのファイルリストを取得
指定のディレクトリ以下にあるファイル全部にアクセスする  
注意点としては現在のディレクトリを示す'.'や親ディレクトリを示す'..'に対して処理をしたくない場合はこれらを除外する処理をおこなう。

    if ($dir = opendir("data/")) {
  		while (($file = readdir($dir)) !== false) {
  		    if ($file != "." && $file != "..") {
  		        echo "$file\n";
  		    }
  		} 
  		closedir($dir);
  	}
    
###正規表現 regular expression
<!-- re:: regex:: -->
    preg_match('/(.+)\.js/', $src_string, $m);
    $m[0] -> ヒットした文字列全体
    $m[1] -> １つ目の()にヒットした文字列
    $m[2] -> ２つ目の()にヒットした文字列

    // 英数半角文字チェック
    $str = "hogehoge";
    if (preg_match("/^[a-zA-Z0-9]+$/", $str)) {
      echo "すべて半角英数である";
    } else {
        echo "すべて半角英数ではない";
    }

###プログラムの引数
<!-- args: argv: arg: -->
  コマンドラインから
  
    $ php test.php arg1 arg2 
  
  を実行した場合、$argv に引数が入る

    $argv[0]  "test.php"
    $argv[1]  "arg1"
    $argv[2]  "arg2"

###乱数 rand
<!-- rand:: -->
  __int rand()__  
  __int rand( int $min, int $max );__  

    echo rand() . "\n";
    echo rand(0,1000) . "\n";
    echo rand(0,1000) . "\n";
  
###ハッシュ hash
<!-- hash:: -->
  文字列をハッシュ化。

    string password_hash ( string $password , integer $algo [, array $options ] )

    ※ハッシュ化された文字列は毎回変わる。このハッシュが元の文字列と一致するかどうかは文字列の単純比較ではできないため、password_hashメソッドを使用する。

    例:
      $hashedStr = password_hash( "hoge", PASSWORD_DEFAULT);

    ハッシュ化した文字列の一致比較:
    boolean password_verify ( string $password , string $hash )

    例:
      if (password_verify("hoge", $hashedStr))

###ファイルのパスを取得する
// pathinfo を使用する

~~~
$file_info = pathinfo('web/public_html/index.php');
 
echo $file_info['dirname']."\n";    // ディレクトリ名
echo $file_info['basename']."\n";    // ファイル名
echo $file_info['extension']."\n";    // 拡張子
echo $file_info['filename'];        // 拡張子を除いたファイル名 ( PHP 5.2.0 以降 )
~~~



#その他 others
  [foreachの$valueを参照で受けると思わぬバグを引き起こすAdd Star](http://d.hatena.ne.jp/pasela/20080527/foreach)

##PHP Query
<!-- phpquery:: -->
htmlをDOMのようなオブジェクトに変換してから、セレクタや便利メソッドを使ってあれこれできる。jQueryのPHP版。

|リンク|説明|
|---|---|
[PHP Query Project](https://code.google.com/archive/p/phpquery/) | PHP Query 本家のドキュメント 
[PHP Query Attributes](https://code.google.com/archive/p/phpquery/wikis/Attributes.wiki) | attr(),html(),text()等の属性系のメソッド 
[DOMElement クラス](http://php.net/manual/ja/class.domelement.php) | PHP の DOM Elementクラス

PHP Query DOMオブジェクト生成
~~~php
<?php
require_once("../Library/phpQuery-onefile.php");

// Get Data Source
$html = file_get_contents("./_swift_memo.html");

// Get DOM Object
$dom = phpQuery::newDocument($html);

// Get 
$tag = $dom['div.hoge'];
print($tag);
?>
~~~

htmlからDOMオブジェクトを生成。いくつか方法がある
~~~php
<?php
// htmlファイルから生成
$dom = phpQuery::newDocumentFile("hoge1.html");

// テキストファイルから生成
$html = get_file_contents("hoge1.html");
$dom = phpQuery::newDocument($html);

// htmlテキストから生成 (newDocumentと同じ？)
$html = get_file_contents("hoge1.html");
$dom = phpQuery::newDocumentHTML($html);
?>
~~~


**DOM Element** オブジェクトと **PHP Query** オブジェクト  
PHP Query オブジェクトは便利関数がいろいろ使えるが、PHP Query を 
PHP の DOM Element オブジェクトはこちら[DOMElementクラス](http://php.net/manual/ja/class.domelement.php) を参照。
~~~php
<?php
// セレクタで取得できるのは PHP Query オブジェクト
$pq1 = $dom['div.top'];

$pq2 = $dom['li'];    // $pq2は PHP Queryオブジェクト
foreach($pq2 as $element) {
  // $element は PHPの DOMElement オブジェクトなのでセレクタや文字列変換はできない
  // print($element);     エラー
  // $element['a'];       エラー
  // $element->attr("name")   エラー

  // pq()で PHP Query オブジェクトに変換してからあれこれする。
  pq($element)['a'];      // OK
  pq($element)->attr("name");   // OK
}
?>
~~~

**セレクタ (Selector)**  
cssやjQueryのセレクタとほとんど同じセレクタが使用出来る。  
[Selectors.wiki](https://code.google.com/archive/p/phpquery/wikis/Selectors.wiki)
~~~php
<?php
// <div class="top"></div> を取得
$tag_top = $dom['div.top'];

// タグ
$tag_p = $tag_top['p'];

// id #
$tag_id = $tag_top['#hoge_id'];
print($tag_id . "\n");

// class .
$tag_class = $tag_top['.hoge'];
print($tag_class . "\n");

// attr
// have
$tag_attr = $tag_top['[href]'];
print($tag_attr . "\n");

// attr=value
$tag_attr = $tag_top['[name=shutaro]'];
print($tag_attr . "\n");

// attr!=value
$tag_attr = $tag_top['[name!=shutaro]'];
print($tag_attr . "\n");

// multiple selector
// selector1,selector2,...
$tag_mul = $tag_top['div.hoge,div.hoge2'];
print($tag_mul . "\n");

// only child (not grandchild)
$tag_child = $tag_top['.ul1 > li'];
print($tag_child . "\n");

// filter
// fist element
$tag_filter = $tag_top['.ul1 > li:first'];
print($tag_filter . "\n");

// last element
$tag_filter = $tag_top['.ul1 > li:last'];
print($tag_filter . "\n");

// first child (自分の親の最初の子要素)
$tag_filter = $tag_top['.ul1 > li:first-child'];
print($tag_filter . "\n");

// parent 親
$tag_filter = $tag_top['.ul2:parent'];
print($tag_filter . "\n");

?>
~~~

**属性 (Attributes)**  
[PHP Query Attributes](https://code.google.com/archive/p/phpquery/wikis/Attributes.wiki)  
~~~php
<?php
$dom1 = $dom['div.hoge'];
// テキストノードを取得
$dom1->text();

// html要素を参照(自分のノードは含まない)
$dom1->html();

// html要素を設定
$dom1->html("<div>hoge</div>");

// 指定の名前の属性
$dom1->attr("name");

// 指定の名前の属性を書き換える
$dom1->attr("name", "new_name");

// 属性を削除
$dom1->removeAttr("name");

// クラスを追加
$dom1->addClass("hoge");
?>
~~~
