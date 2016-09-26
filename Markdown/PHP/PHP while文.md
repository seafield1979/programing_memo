#while文
条件が一致する間は処理を繰り返す

```php
while(条件式){
  処理
}
条件式がtrueの間はずっと処理を行う。

$cnt = 10;
while($cnt > 0){
  echo "cnt=${cnt}\n";
  $cnt--;
}
```