#switch文
Cとほぼ一緒だが、caseの値に文字列が使用出来る。が、文字列の比較に==演算子を使っているので100と"100"が同じとみなされるため注意。

```php
switch ($i) {
case 0:
    echo "iは0に等しい";
    break;
case 1:
    echo "iは1に等しい";
    break;
case 2:
    echo "iは2に等しい";
    break;
default:
   echo "iは0,1,2に等しくない";
}

// caseに文字列が使える
switch ($str) {
case "hoge":
    echo "str は hoge";
    break;
case "mage":
    echo "str は mage";
    break;
default:
   echo "hogeでもmageでもない";
}
```