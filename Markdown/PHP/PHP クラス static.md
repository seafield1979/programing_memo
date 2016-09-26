###static
staticをつけたプロパティやメソッドはクラスのインスタンスなしに呼び出すことができる。C++で言うところのclass変数、classメソッド。

```php
Class CTest1 {
  public static $name = "SHUTARO";
  public static function static_func(){
    echo self::$name . "\n";
  }
}

// staticプロパティの参照
echo CTest1::$name . "\n";
// staticメソッドを呼び出す
CTest1::static_func();
s

```