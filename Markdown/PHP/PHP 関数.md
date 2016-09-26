#関数 function

+ PHPでは関数呼び出しの後に関数の宣言を定義してもOK
+ PHPでは関数がグローバルスコープにある
+ 再起呼び出しが可能

###基本  

```php
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
```

###動的な関数の有効化
関数の中に関数定義を行うことで、外の関数が呼び出された後に中の関数が利用可能になる。

```php
<?php
function func1(){
    function func2(){
        echo "func2";
    }
}
// func2();  未定義エラー

func1();        // func1のfunc2が有効になる
func2();        // ok
```

###引数の参照渡し
関数の引数を参照型にすることで、関数内で引数に渡した変数や配列の内容を変更することができる

```php
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
```