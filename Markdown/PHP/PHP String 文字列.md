#文字列 string

文字列の連結は`'.'`  
	`"hoge" . "hoge" . $str1`

###シングルコート文字列 

```php
//シングルコード文字列はエスケープ(\nや\t)や変数は展開されない
// '文字列'
echo 'hello\n'       // hello\n  と表示される
echo 'hello $hoge'   // hello $hoge と表示される
```

###ダブルコート文字列

```php
// ダブルコートで囲んだ文字列内のエスケープや変数は展開される
// "文字列"
echo "hello\n";     // hello の後に改行される
echo "hello$hoge";  // $hogeが展開されて表示される
echo "hello${hoge}"   // $hogeが展開されて表示される。上と同じ


```

###文字列の要素にアクセス
ある文字列を１文字づつ取り出して処理を行う方法

```php
$hoge = "hoge";
echo $hoge[0];    // h
foreach (str_split($hoge) as $value){
  echo $value . "\n";   // h,o,g,e が出力される
});


```

###文字列操作メソッド

```php
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
$str2 = substr("hogehoge123", 4);   // $str2 = "hoge123"分割

// 指定文字列で分割
// 文字列内にある特定の文字列で分割する
// array explode ( string $delimiter , string $string [, int $limit = PHP_INT_MAX ] )
// 例:  aaa,bbb,ccc 
$array = explode(",", "aaa,bbb,ccc", )
~~~
```

###エスケープ文字
  ダブルコート内で使用出来るエスケープ。エスケープを使用すると改行やタブ、特殊文字を出力できたりする。

|文字|意味|
|---|---|
|\n| 改行
|\t| tab
|`\$` | $
|`\\`| \
|\"|  "


###ヒアドキュメント 
  複数行に渡る文字列をそのままの形で書くことができる。
<<<[適当な文字列]  
設定したい文字列  
[適当な文字列];  

```php
例:
echo <<<hoge
  aaa
  bbb
  ccc
hoge;
```