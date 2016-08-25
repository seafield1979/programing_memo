<link href="http://jasonm23.github.com/markdown-css-themes/markdark.css" rel="stylesheet"></link>

phpメモ

[TOC]

##基本
    htmlの中にphpの処理を書く場合は
    ・ファイル名を .php にする
    ・html内のphp処理を <?php ?> で囲む
    とすると、sublimeでhtmlのタグもphpの公文も色分けされるので見やすい。

##インストール
Linux::  
    ルートで作業する  
~~~sh
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


##ファイルパス
Mac
	/private/etc/php.ini
Windows

Linux

phpファイルの置き場所  
/Library/WebServer/Documents


##対話モード
  Windowsでは
  php -a 
  でインタラクティブモードを起動し、
    echo "hello world";
    と入力しても何も表示されない。

    インタラクティブモードで処理を実行させるには
    <?php
      echo "hello world";
    ?>
    などと <?php ?> で処理を囲ってから Ctrl + Z を入力する必要がある。

    
##hello world
<!-- hello:   -->
htmlファイル内でPHPプログラムを動作させてみる

~~~html
php_test.html

<html>
 <head>
  <title>PHP Test</title>
 </head>
 <body>
 <!-- ここがPHPプログラム
    <?php echo '<p>Hello World</p>'; ?> 
 -->
 </body>
</html>
~~~

##PHPのログを出力する log: logs:
　php.iniの設定
~~~ini
#htmlにエラーを出力するかどうか
display_errors = On/Off
  
###ログの出力レベル
error_reporting = E_ALL | E_STRICT

#ファイルに出力
log_errors = On/Off

#出力ファイルのパス
error_log = /var/log/php/error.log
~~~

###ログの出力(tailを使用して常に監視)  
`$tail -50f /var/log/php/error.log`

※php.iniの変更を反映させるためにはApacheをリスタートする  
`$sudo apachectl restart`


##MySQLでデータベースを捜査する
  既にphpとMySQLが使用できる状態にしてある前提で進める
  phpで以下のメソッドを使用する

__接続__:
    mysql_connect
~~~
resource mysql_connect([string server [, string username [, string password [, bool new_link [, int client_flags]]]]])

例: $link = mysql_connect('localhost', 'root', 'poco');
~~~
__切断__:
  mysql_close
~~~
bool mysql_close([resource link_identifier])

例: $ret = mysql_close($link);
~~~
  ・mysqlに接続したら各種メソッドを使用してデータベースに要求を出す
    http://php.net/manual/ja/ref.mysql.php

##phpでMySQLを使用する MySQL: db:
  phpにはMySQL接続のためのAPIが３種類用意されてある。  
  mysql と mysqli そして PDO  
  mysqlは古いので使用しない方がよいらしい。  
	[どの API を使うか](http://php.net/manual/ja/mysqlinfo.api.choosing.php)

sample:: mysqli

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

##文字列表示 print:
~~~php
<?php
//echo:
  $str = "hello world!!";
  echo "hogehoge" . $str1 . "\n";

//print_r:
  見やすい形に成形して文字列出力する
  $array1 = array("1","2");
  print_r($array);
  // 出力
    Array
    (
        [0] => 1
        [1] => 2
    )
//var_dump:
  $str = $var_dump($array1);
~~~

##基本 basic:

~~~php
<?php
  // 一行コメントアウト
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

##変数 var:
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

###変数の型:  
|型|説明|
|!--|!--|
  **boolean**| true / false をとる。大文字、小文字は区別しない  
  **整数**| 整数値 32bit環境なら -2147483648 ~ 2147483647  
  **浮動小数点**| 1.2345 みたいな少数の値  
  **文字列**|  
  **配列**|  
  _オブジェクト_|  
  _NULL_|  

##イクルード
include:  
require:  
require_once:  

    外部ファイルを読み込む。ファイルパスを指定しない場合は php.ini の include_path で指定したパスを検索する。
    includeは致命的なエラーを発生させないが、requireは致命的なエラーが発生してスクリプトが停止する可能性がある。
      include 'hoge.php';   //パスを指定しない php.ini の include_path を探す
      include './hoge.php'; //パスを指定

    使い分け
      includeとrequireの使い分け
        http://furudate.hatenablog.com/entry/2013/10/17/141557
      include  ページの内容を差し込みたい
      require  PHPの処理を読み込みたい。同じ処理を何度も読み込む必要はないので、通常は require_once を使う。

##定数 const:
~~~php
<?php
    //define(識別子, 値);
    define("TAX", 0.08);
    define(TAX2, 0.10);   // ダブルコーテーションなしでもOK
?>
~~~

### 自動的に定義される定数
|定数名|説明|
|!--|!--|
  `__LINE__` | ファイル上の現在の行番号。
  `__FILE__` | ファイルのフルパスとファイル名 (シンボリックリンクを解決した後のもの)。 インクルードされるファイルの中で使用された場合、インクルードされるファイルの名前が返されます。
  `__DIR__` | そのファイルの存在するディレクトリ。include の中で使用すると、 インクルードされるファイルの存在するディレクトリを返します。 つまり、これは dirname(`__FILE__`) と同じ意味です。 ルートディレクトリである場合を除き、ディレクトリ名の末尾にスラッシュはつきません。
  `__FUNCTION__` | 関数名。
  `__CLASS__` | クラス名。 クラス名には、そのクラスが宣言されている名前空間も含みます (例 Foo\Bar)。 PHP 5.4 以降では、__CLASS__ がトレイトでも使えることに注意しましょう。トレイトのメソッド内で __CLASS__ を使うと、そのトレイトを use しているクラスの名前を返します。
  `__METHOD__` | クラスのメソッド名。
  `__NAMESPACE__` | 現在の名前空間の名前。


##文字列
<!-- string:: -->
文字列の連結は`'.'`  
	`"hoge" . "hoge" . $str1`

####シングルコート文字列 

    シングルコード文字列はエスケープ(\nや\t)や変数は展開されない
    '文字列'
    echo 'hello\n'       // hello\n  と表示される
    echo 'hello $hoge'   // hello $hoge と表示される

####ダブルコート文字列

    ダブルコートで囲んだ文字列内のエスケープや変数は展開される
    "文字列"
    echo "hello\n";     // hello の後に改行される
    echo "hello$hoge";  // $hogeが展開されて表示される
    echo "hello${hoge}"   // $hogeが展開されて表示される。上と同じ

####文字列の要素にアクセス
ある文字列を１文字づつ取り出して処理を行う方法

    $hoge = "hoge";
    echo $hoge[0];    // h
    foreach (str_split($hoge) as $value){
      echo $value . "\n";   // h,o,g,e が出力される
    });

####文字列の長さ strlen:
    strlen(文字列)
    ※文字列以外の値を渡すとエラーになる。is_stringでチェックが必要

####文字列に変換 strval:
    $変数 = strval(123);    // 変数には返還後の文字列"123"が入る

####パターンマッチ preg_match:
  preg_match(パターン, 対象文字列, 結果が格納される変数);

    preg_match('/(.+)\.js/', $src_string, $m);
    $m[0] -> ヒットした文字列全体
    $m[1] -> １つ目の()にヒットした文字列
    $m[2] -> ２つ目の()にヒットした文字列

### 異なる型の加算
数値と文字列の加算(+)は、文字列が数値として評価できた場合にのみ数値として加算される。

    $foo = 1 + "10.5";              // $foo は float です (11.5)
    $foo = 1 + "-1.3e3";            // $foo は float です (-1299)
    $foo = 1 + "bob-1.3e3";         // $foo は integer です (1)  
    $foo = 1 + "bob3";              // $foo は integer です (1)  後の文字列は無視される

##エスケープ文字
  ダブルコート内で使用出来るエスケープ。エスケープを使用すると改行やタブ、特殊文字を出力できたりする。

    \n 改行
    \t tab
    \$ $
    \\ \
    \"  "

##ヒアドキュメント 
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

##演算子 operator:
####四則演算
    +,-,*,/
    ++    インクリメント。１増やす
    --    デクリメント。１減らす

#####比較演算子
  |演算子|説明|
  |!--|!--|  
  == |   型を相互変換した上で値が同じか
  != |  型を相互変換した後で値が同じでないか
  === |  値と型が同じか
  !== |  値と型のどちらかが同じでないか

####論理演算子
  |演算子|説明|
  |!--|!--|  
  ! |    not  if(!true) で falseになる 
  && |   条件1 && 条件2  で両方がtrueでtrue
  &#124;&#124; |   条件1 &#124;&#124; 条件2  でどちらかがtrueでtrue

##配列 array:
  PHPの配列はすべて連想配列。2次元配列でも子要素の要素数はバラバラにできる

####初期化
~~~php
  // 空の配列
  $配列名 = array();

  // キー名 & 値 [key1]=>value1, [key2]=>value2, ...
  $配列名 = array(key1=>value1, key2=>value2, ...);

  例:  $array1 = array(a=>1, b=>2, c=>3);
      $array2 = array("a"=>1, "b"=>2, "c"=>3);

  // キー名を省略 [0]=>value1, [1]=>value2, ...
  $配列名 = array(value1, value2, ...);
  例:  $array1 = array("hoge", "hoge2", "hoge3");
~~~

####配列の参照:
    配列の要素を参照する場合は
      $配列名["キー"]; 

    キーなしで配列を宣言した初期化した場合は 0 から要素が入っていく
      $array = array(1,2,3,4,5);    [0]=>1, [1]=>2, [2]=>3, [3]=>4, [4]=>5

    例: $array = array(1,2,3,4,5);
        echo $array[0];   // 1
        echo $array[2];   // 3

####要素数を取得: サイズ
    count($配列);

####配列の中に配列:
    $array1 = array("hoge", array("hoge21", "hoge22", array("hoge31", "hoge32")));
####配列を表示:
    echo var_dump($配列);
    print_r($配列);

####要素の追加:
    // キーを指定する方法
    $arr[キー] = 値;
    
    // 配列の最後に追加
    $arr[] = 値;

####要素の削除: unset:
    $array = array(a=>1, b=>2, c=>3);
    unset($array[b]);

####要素が存在するかどうかを判定する: array_key_exists:
    $ret = array_key_exists($配列[キー]);
    $array=(a=>1,b=>2,c=>3);
    array_key_exists($array[a]);    // true
    array_key_exists($array[e]);    // false

####全要素のアクセス: foreach:
    $array = array(a=>1, b=>2, c=>3, d=>4);
 
    // 値
    foreach($array as $value) {
        echo "value=$value\n";
    }
    // キーと値
    foreach($array as $key=>$value) {
        echo "key=$key value=$value\n";
    }

##変数の型判定　gettype::
変数の型を判定する方法  
###gettype(値 or 変数);
 
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

####各種メソッドを使って判定する
  get_class() - オブジェクトのクラス名を返す
  is_array() - 変数が配列かどうかを検査する
  is_bool() - 変数が boolean であるかを調べる
  is_callable() - 引数が、関数としてコール可能な構造であるかどうかを調べる
  is_float() - 変数の型が float かどうか調べる
  is_int() - 変数が整数型かどうかを検査する
  is_null() - 変数が NULL かどうか調べる
  is_numeric() - 変数が数字または数値形式の文字列であるかを調べる
  is_object() - 変数がオブジェクトかどうかを検査する
  is_resource() - 変数がリソースかどうかを調べる
  is_scalar() - 変数がスカラかどうかを調べる
  is_string() - 変数の型が文字列かどうかを調べる
  function_exists() - 指定した関数が定義されている場合に TRUE を返す
  method_exists() - クラスメソッドが存在するかどうかを確認する
  
##if文 if:
  
    if(条件){
    } elseif(条件2) {
    } else {
    } 

    条件には以下のような常にtrue,falseになる書き方ができる
    if (true) or (1)     // 常にtrue
    if (false) or (0)    // 常にfalse


##loop:
####for
    for ($cnt=0; $cnt < 10; $cnt++) {
        echo "cnt=${cnt}\n";
    }

####foreach
<!-- foreach:: -->

    $array = array(name=>"shutaro", age=>36, comment=>"hoge");
    foreach ($array as $key=>$value) {
      echo "key=${key} value=${value}\n";
    }

####while
<!-- while:: -->

    while(条件式){
      処理
    }
    条件式がtrueの間はずっと処理を行う。

    $cnt = 10;
    while($cnt > 0){
      echo "cnt=${cnt}\n";
      $cnt--;
    }

##switch文
<!-- switch:: -->
  Cとほぼ一緒

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

##enum
  PHPでは言語ではenumをサポートしていないため、リフレクションを使って実装する  
  [PHPで列挙型(enum)を作る](http://qiita.com/Hiraku/items/71e385b56dcaa37629fe)

##関数
 <!-- func: function: -->

+ PHPでは関数呼び出しの後に関数の宣言を定義してもOK
+ PHPでは関数がグローバルスコープにある
+ 再起呼び出しが可能

####宣言  

    fucntion 関数名($引数1, $引数2, ...){
        // 処理
        return 戻り値;
    }

    function foo($arg_1, $arg_2, /* ..., */ $arg_n)
    {
        echo "関数の例\n";
        return $retval;
    }


####呼び出し
  $戻り値 = 関数名(引数1, 引数2, ...);  
  $戻り値 = @関数名(引数1, 引数2, ...);   
    先頭に@をつけるとエラーが出力されなくなる

####動的な関数の有効化
  関数の中に関数定義を行うことで、外の関数が呼び出された後に中の関数が利用可能になる。

    function func1(){
        function func2(){
            echo "func2";
        }
    }
    // func2();  未定義エラー

    func1();        // func1のfunc2が有効になる
    func2();        // ok

####引数の参照渡し
  関数の引数を参照型にすることで、関数内で引数に渡した変数や配列の内容を変更することができる

    // 参照渡しにしたい変数の先頭に & をつける
    function hoge(&count) {
      $count++;
    }
    $count1 = 10;
    hoge($count1);
    echo $count1;   // hogeメソッドの中でインクリメントされたので 11 になる
    
##クラス
<!-- class: object: -->
PHPのクラスは以下のような機能を持つ。
※オブジェクトはクラスから生成された実態。インスタンスともいう。
  
####クラスの定義:
    class クラス名 {
      public $プロパティ = 初期値;
      private $プロパティ = 初期値;
      public function クラス(){
        // 処理
      }
    }

####インスタンス生成:
    $instance = new クラス名();
    もしくは
    $instance = new クラス名;
####メンバ参照:
$instance->プロパティ変数;

    $hoge = new Hoge();
    $hoge->name;
####メソッド呼び出し:
    $instance->メソッド();

####インスタンスの参照:  
  生成済みのインスタンスを別の変数に代入すると、代入先の変数は代入元の変数の参照になる。
  参照なので実態は１つだけ。よって参照元、参照先の変更はどちらにも反映される。

    $hoge = new CHoge();
    $hoge2 = $hoge;
    $hoge3 = & $hoge;

####クローン
<!-- clone: -->
  PHPではインスタンス変数の代入は参照を作るだけなので、別のインスタンスを作りたい場合は clone を使用する

    $hoge = new CHoge();
    $hoge2 = clone $hoge;
    $hoge->name = "shutaro";
    $hoge2->name = "naotaro";
    // これで $hoge->name と $hoge2->name の値はそれぞれ別の値になる。

  クローン時に実行されるメソッドを定義できる

    class Hoge{
      public $id = 0;
      function __clone(){
        ~クローン時の処理~
        個別のインスタンスで別々の値を持ちたいときなどに、ここで新たに確保する
      }
    }

####アクセス制 public: private: protected:
  publicはクラスの外から参照可能  
  privateはクラスの外から参照不可  
  protectedは定義したクラスと継承したクラスからのみ参照可能  

#####static:
  staticをつけたプロパティやメソッドはクラスのインスタンスなしに呼び出すことができる。C++で言うところのclass変数、classメソッド。
  
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

#####finalキーワード:
  final メソッドは継承先のクラスで上書きできない。  
  final クラスは継承できない。  

    final public function hoge(){
      // 子クラスで上書き不可
    }

  final class CHoge{
    // 拡張（継承）不可  
  }

####オブジェクト定数:
  変更できない定数。インスタンスなしでアクセス可能なのでstatic変数に似ている。
  
    class MyClass
    {
        const CONSTANT = 'constant value';

        function showConstant() {
            echo  self::CONSTANT . "\n";
        }
    }
    echo MyClass::CONSTANT . "\n";
  

####コンストラクタとデストラクタ
<!-- construct: destruct: -->

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

####継承: extends:
  PHPでは継承先のクラスは継承元の public,protectedのプロパティ、メソッドを引き継ぐ。privateは引き継がない。
  子クラスから親クラスのメソッドを呼ぶには parent::メソッド名() を使用する。

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

<!-- ■標準クラス standard:: -->

##例外処理 exception:
  PHPもC＋＋と同じ感じで例外をキャッチできる。

    try {
      // 例外が発生するかもしれない処理を try スコープに入れる
      throw new Exception("error1");
    } catch (Exception $e){
      echo $e->getMessage . "\n";
    } finnaly {
      // 必ず実行される処理
    }
  Exceptionを継承した自前の例外クラスを使う
  
    class MyException extends Exception { }
    class MyException2 extends Exception { }

    class Test {
        public function testing() {
            try {
                try {
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
  
■名前空間 namespace::
  
  [PHPの名前空間について簡単にまとめてみた](http://ucwd.jp/blog/736)

  独自の名前空間を使用しない場合、宣言したメソッドやクラス、グローバル変数はすべて同じ名前空間に入るため、名前がかぶってエラーになってしまう。
  名前がかぶらないようにするためには、必要に応じてファイル単位で別々の名前空間を宣言する必要がある。
  
  namespace1.php

    namespace NS1;
    function hoge(){
      echo "hoge1\n";
    }

  namespace2.php

    namespace NS2;
    // namespace1.php にある関数 hoge と同じ名前
    function hoge(){
      echo "hoge2\n";
    }

  test.php
    require_once "./namespace1.php";
    require_once "./namespace2.php";

    hoge();
    NS1/hoge();
    NS2/hoge();
  
##エイリアスとインポート:
  名前空間を指定してメソッドを呼び出す場合に  
    `\My\Space\Is\Very\Small\echo_message();`  
  のように長い名前空間をつけるのは煩雑になるので、エイリアスとインポートを使って記述を短縮することができる。

####エイリアス: aslias:
    名前空間名を別の名前でも呼び出せるようにする
    -- use 元の名前空間 as 別名の名前空間
    require_once 'file02.php';
    use My\Space\hoge\hage\Anpontan as MySpace;

    MySpace\echo_message();
    $obj = new MySpace\MySpaceClass();

####インポート: import:
  `use 元の名前空間 + その名前空間に含まれるクラス名 as 別名のクラス名`  
  名前空間 My\Space に属する MySpaceClass クラスに直接エイリアスを定義し、且つその別名で My\Space\MySpaceClass を呼び出せる

    require_once '20130806_02.php';

    use My\Space\MySpaceClass as MyClass;
    $obj01 = new MyClass();


##ファイル操作

####開くfopen
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
####閉じる fclose:
    bool fclose( resource handle )
####１行読み込む fgets:
    string fgets( resource handle [, int length ] )
    ファイルポインタから 1 行取得

    while( ! feof( $fp ) ){
      echo fgets( $fp, 9182 );
    }

####１件読み込む fread:
    string fread ( resource $handle , int $length )
    handle が指すファイルポインタから最高 length バイト読み込む

    戻り値:
    ファイルポインタから読み出した文字列／FALSE（ファイルが終端に達したかエラーの場合）

####１件書き込む fwrite: fputs:
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
  
####全行を配列に読み込む:  file():
  
    array file( string filename [, int use_include_path ] )
    
    戻り値:
    ファイルのデータが格納された配列／FALSE（失敗した場合）

####ファイルの内容を読み込む file_get_contents:
    ファイルの内容をすべて取得する
    string file_get_contents( string filename [, bool use_include_path ] )

    例:
    $str = file_get_contents("./hoge.txt");

####ファイルから読み込む
    // hoge.txt から全行を読み込む
    if (!($fp = @fopen("hoge.txt", "r"))) {
      exit("failed to open file\n");
    }
    while( ! feof( $fp ) ){
      echo fgets( $fp, 9182 );
    }

####ファイルに書き込む
    // hoge.txt に "hello world"を書き込む
    if(!($fp = fopen("hoge.txt", "w+"))) {
      exit("file open error");
    }
    fputs($fp, "hello world");
    fclose($fp);

##データベース database:
<!-- DB: database: -->

##セッション session:
セッションは一連の接続を管理する仕組み。httpはステートレスなので、通常は連続でページを読み込んでも以前の状態は保持されない。そこで、クライアント側のcookieにセッションIDを保存しておき、サーバー側でこのセッションIDを参照することでユーザーの状態を把握する。
PHPの提供するセッションの仕組みは  
  session_start()　でセッション開始、セッション再開
  セッションが始まると $_COOKIE["PHPSESSID"] にセッションIDが入る
  $_SESSION にセッション変数の保持、ただしセッションが終わると参照できなくなる。
  session_destroy() でセッションを終了。

####セッションを開始する:
    session_start();  
  htmlファイルの先頭に記述する。おまじないのようなもの
    新しいセッションを開始、あるいは既存のセッションを再開する
  これでCookieに "PHPSESSID" というセッションIDが入ったデータが作成される。

####セッションIDの参照:
    $_COOKIE["PHPSESSID"];  
    //687301552c9e5bd0393ccbf691173d7d  のようなランダムな文字列
    
####セッション変数の登録:
    そのセッションでだけ有効な変数
    $_SESSION[キー] = 値

####セッッションを終了する:
  session_destroy();


##その他 other:
escape:: htmlで入力された文字をエスケープ
###HTMLエスケープ:
    htmlspecialchars('you & I > he & she');
    // 結果: you &amp; I &gt; he &amp; she
  
###HTMLアンエスケープ:
    htmlspecialchars_decode('you &amp; I &gt; he &amp; she');
    // 結果: you & I > he & she
    
###Cookie::
####Cookieを保存:
    setcookie( cookieName , value , [timeout] , [path] , [domain] )
    
    cookieName:  Cookie名
    value:      Cookie値
    timeout:  ※省略可能。Cookieの有効期限。このCookieが有効となる期限を秒単位で指定します。
            例）0 … ブラウザが閉じられた時点で削除されます。
            例）time() + (30 * (24*60*60)) … 30日間有効になります。
            　　time()で、現在時刻が『1970年1月1日 00:00:00 GMT』からの
            　　通算秒として返されます。
            　　そこに30日×(24時間×60時間×60秒)を加算して算出しています。
    path:  ※省略可能。Cookieが有効なパス。
          例）"/"
    domain:  ※省略可能
          Cookieが有効なドメイン。
          例）"www.24w.jp"

####Cookieを取得
    var value = $_COOKIE[ cookieName ]
    例:
      $hoge = $_COOKIE["hoge"];

####Cookieを削除:
    setcookie( "クッキー名", "", time()-10000);
    // timeに過去の日付を設定すると削除される。
  
    
###リダイレクト　redirect:
  header('location: [パス]');
  ※html本体より前に送信しないといけない。よってheaderの前に echo 文で文字列を出力していたりすると失敗する

###日時時刻取得 date:
    date("Y-m-d H:i:s");

    string date ( string $format [, int $timestamp = time() ] )
    format:
      d 日。二桁の数字（先頭にゼロがつく場合も）  01 から 31
      D 曜日。3文字のテキスト形式。  Mon から Sun
      l (小文字の 'L')  曜日。フルスペル形式。 Sunday から Saturday
      w 曜日。数値。  0 (日曜)から 6 (土曜)
      月 --- ---
      F 月。フルスペルの文字。 January から December
      m 月。数字。先頭にゼロをつける。 01 から 12
      M 月。3 文字形式。 Jan から Dec
      t 指定した月の日数。 28 から 31
      年 --- ---
      Y 年。4 桁の数字。 例: 1999または2003
      y 年。2 桁の数字。 例: 99 または 03
      時 --- ---
      a 午前または午後（小文字）  am または pm
      A 午前または午後（大文字）  AM または PM
      h 時。数字。12 時間単位。 01 から 12
      H 時。数字。24 時間単位。 00 から 23
      i 分。先頭にゼロをつける。  00 から 59
      s 秒。先頭にゼロをつける。

```php
<?php
      date_default_timezone_set('Asia/Tokyo');    // タイムゾーンを設定しないとグリニッジ標準時間になる
      $date_str = date("Y-m-d H:i:s");
      echo $date_str, "\n";
?>
```

####フォーマットで日付取得
    $date_str = date("Y-m-d H:i:d");
    $next_day = date("Y-m-d", strtotime("+1 day"));

###型変換	convert:
    文字列 -> 整数値
    戻り値 = (int)文字列;  // キャスト
    戻り値 = intval(文字列);  // 関数
	
###置換 replace:
  	str_replace(置換元, 置換先, 対象文字列)
  	str_replace('-', '/', $aaa); 		'-' -> '/'に変換

    置換元(文字列か文字列の配列)
    置換先(文字列か文字列の配列)

    置換元と置換先が両方とも文字列の場合  ただの置換、置換後の文字列を返す
    置換元が配列で置換先が文字列の場合  置換元の配列全部に対して置換先の文字列で置換、配列を返す
    置換元と置換先が両方とも配列の場合  それぞれの配列から順に文字列を取り出し置換を行う。配列を返す


###POST:: GET::
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
    
###正規表現 re: regular expression:
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

###プログラムの引数 args: argv: arg:
  コマンドラインから
  
    $ php test.php arg1 arg2 
  
  を実行した場合、$argv に引数が入る

    $argv[0]  "test.php"
    $argv[1]  "arg1"
    $argv[2]  "arg2"

###乱数 rand:
  __int rand()__  
  __int rand( int $min, int $max );__  

    echo rand() . "\n";
    echo rand(0,1000) . "\n";
    echo rand(0,1000) . "\n";
  
###ハッシュ hash:
  文字列をハッシュ化。

    string password_hash ( string $password , integer $algo [, array $options ] )

    ※ハッシュ化された文字列は毎回変わる。このハッシュが元の文字列と一致するかどうかは文字列の単純比較ではできないため、password_hashメソッドを使用する。

    例:
      $hashedStr = password_hash( "hoge", PASSWORD_DEFAULT);

    ハッシュ化した文字列の一致比較:
    boolean password_verify ( string $password , string $hash )

    例:
      if (password_verify("hoge", $hashedStr))

###その他
  [foreachの$valueを参照で受けると思わぬバグを引き起こすAdd Star](http://d.hatena.ne.jp/pasela/20080527/foreach)

