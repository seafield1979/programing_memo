#コンストラクタとデストラクタ construct destruct
コンストラクタはオブジェクトが生成される時、デストラクタはオブジェクトが破棄される時に呼ばれるメソッド。コンストラクタではプロパティの初期化やリソースの読み込み等を行い、デストラクタではリソースの解放等を行う。

```php
// コンストラクタ
function __construct(){}

// デストラクタ
function __destruct(){}
```

```php
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
```