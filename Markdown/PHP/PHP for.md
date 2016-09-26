#ループ処理 loop

###for

```php
for ($cnt=0; $cnt < 10; $cnt++) {
    echo "cnt=${cnt}\n";
}
```

###foreach

```php
// 基本
$array = array(name=>"shutaro", age=>36, comment=>"hoge");
foreach ($array as $key=>$value) {
  echo "key=${key} value=${value}\n";
}

// ループの最初と最後を取得
$array = array(name=>"shutaro", age=>36, comment=>"hoge");
foreach ($array as $key=>$value) {
  if ($value == reset($array)) {
    // 最初
  }
  else if ($value == end($array)) {
    // 最後
  }
  echo "key=${key} value=${value}\n";
}
```