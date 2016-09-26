#クラス定数 constant
変更できない定数。インスタンスなしでアクセス可能なのでstatic変数に似ている。

```php
class MyClass
{
    const CONSTANT = 'constant value';

    function showConstant() {
        echo  self::CONSTANT . "\n";
    }
}
echo MyClass::CONSTANT . "\n";
```