#名前空間 namespace

[PHPの名前空間について簡単にまとめてみた](http://ucwd.jp/blog/736)
  独自の名前空間を使用しない場合、宣言したメソッドやクラス、グローバル変数はすべて同じ名前空間に入るため、名前がかぶってエラーになってしまう。名前がかぶらないようにするためには、必要に応じてファイル単位で別々の名前空間を宣言する必要がある。

namespace1.php

```php
namespace NS1;
// 以下のメソッドは名前空間 NS1 に属する
function hoge(){
  echo "hoge1\n";
}
```

namespace2.php

```php
namespace NS2;

// 以下のメソッドは名前空間 NS2 に属する
// namespace1.php にある関数 hoge と同じ名前
function hoge(){
  echo "hoge2\n";
}
```

test.php

```php
srequire_once "./namespace1.php";
require_once "./namespace2.php";

//hoge();   エラー Call to undefined function hoge()
NS1/hoge();
NS2/hoge();
```