#乱数 rand
乱数の生成にはrandメソッドを使用する

~~~
// 0 ~  getrandmax() の乱数を返す
__int rand()__  

// min から max の乱数を返す
__int rand( int $min, int $max );__  
~~~

```php
echo rand() . "\n";
echo rand(0,1000) . "\n";
echo rand(0,1000) . "\n";
```