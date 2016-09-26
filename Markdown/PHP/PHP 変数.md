#変数 variable

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

~~~php
例: プログラムの引数 $argv を使用する
function hoge{
  global $argv;
  print_r($argv);
}
~~~
##変数の型  
|型|説明|
|:--|:--|
|**boolean**| true / false をとる。大文字、小文字は区別しない  
|**整数**| 整数値 32bit環境なら -2147483648 ~ 2147483647  
|**浮動小数点**| 1.2345 みたいな少数の値  
|**文字列**| "hoge" のような文字列 String
|**配列**| 複数の要素を持つ。PHPの配列は全てキーをもとに要素の値を取得する辞書型。
|_オブジェクト_|  _NULL_|  

##型変換・キャスト

~~~php
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

|キャスト|説明|
|---|---|
|(int), (integer) | 整数へのキャスト
|(bool), (boolean) | 論理値へのキャスト
|(float), (double), (real) | float へのキャスト
|(string) | 文字列へのキャスト
|(array) | 配列へのキャスト
|(object) | オブジェクトへのキャスト
|(unset) | NULL へのキャスト (PHP 5)

###変数の型判定　gettype
変数の型を判定する方法 
gettypeメソッドは変数の型を返す。


`echo gettype("hoge");   // string `


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


###各種メソッドを使って判定する
変数の型やnullかどうかなどのチェックを行う便利関数が用意されている。

|メソッド名|説明|
|---|---|
|get_class() | オブジェクトのクラス名を返す
|is_array() | 変数が配列かどうかを検査する
|is_bool() | 変数が boolean であるかを調べる
|is_callable() | 引数が、関数としてコール可能な構造であるかどうかを調べる
|is_float() | 変数の型が float かどうか調べる
|is_int() | 変数が整数型かどうかを検査する
|is_null() | 変数が NULL かどうか調べる
|is_numeric() | 変数が数字または数値形式の文字列であるかを調べる
|is_object() | 変数がオブジェクトかどうかを検査する
|is_resource() | 変数がリソースかどうかを調べる
|is_scalar() | 変数がスカラかどうかを調べる
|is_string() | 変数の型が文字列かどうかを調べる
|function_exists() | 指定した関数が定義されている場合に TRUE を返す
|method_exists() | クラスメソッドが存在するかどうかを確認する


## 異なる型の加算
数値と文字列の加算(+)は、文字列が数値として評価できた場合にのみ数値として加算される。

~~~php
＄foo = 1 + "10.5";              // $foo は float です (11.5)
＄foo = 1 + "-1.3e3";            // $foo は float です (-1299)
＄foo = 1 + "bob-1.3e3";         // $foo は integer です (1)  
＄foo = 1 + "bob3";              // $foo は integer です (1)  後の文字列は無視される
~~~