#文字列表示 print

コンソール(標準出力)に文字列を出力する
`print "文字列"`

`echo "文字列"`

配列やオブジェクトを見やすい形に整形して出力する。
`print_r(配列やオブジェクト)`

`var_dump(配列やオブジェクト)`

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
 
// 標準出力を指定のファイル test.txt に出力
ob_start(); //バッファリング開始
echo "これはテスト用文字列です。";   //ここでは出力されない
$output = ob_get_contents();        //出力されるはずだったデータを取得
ob_end_clean();                     //本来出力されるはずのデータをクリア(これをしないと実行終了時に出力されてしまう)

//上で取得した$outputをファイル(test.txt)に出力する
$fp = fopen('test.txt', 'w');
fwrite($fp, $output);
fclose($fp);
~~~