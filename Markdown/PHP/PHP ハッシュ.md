#ハッシュ hash

文字列をハッシュ化。
[password_hash](http://php.net/manual/ja/function.password-hash.php)

```php
string password_hash ( string $password , integer $algo [, array $options ] )
```

※ハッシュ化された文字列は毎回変わる。このハッシュが元の文字列と一致するかどうかは文字列の単純比較ではできないため、password_hashメソッドを使用する。

**ハッシュを作成**

```php
$hashedStr = password_hash( "hoge", PASSWORD_DEFAULT);
```

**ハッシュ化した文字列の一致比較**

```php
boolean password_verify ( string $password , string $hash )
```

```php
if (password_verify("hoge", $hashedStr)) {
  echo "same";
} else {
  echo "differ";
}
```