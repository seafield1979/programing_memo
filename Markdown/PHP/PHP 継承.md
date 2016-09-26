##継承 extends
<!-- extends:: -->
クラスの継承を行うことで継承元のクラス(親クラス)の機能をそのまま引き継いだ子クラスを定義することができる。  
PHPでは継承先のクラスは継承元の public,protectedのプロパティ、メソッドを引き継ぐ。privateは引き継がない。
  子クラスから親クラスのメソッドを呼ぶには parent::メソッド名() を使用する。

```php
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
```