#配列 array
  PHPの配列はすべて連想配列。2次元配列でも子要素の要素数はバラバラにできる

**初期化**
~~~php
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