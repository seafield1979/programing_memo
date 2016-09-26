#escape
htmlで入力された文字をエスケープ。URLで使用不可能な文字列を特殊な文字列に置き換える。

```php
//HTMLエスケープ
htmlspecialchars('you & I > he & she');
// 結果: you &amp; I &gt; he &amp; she

//HTMLアンエスケープ
htmlspecialchars_decode('you &amp; I &gt; he &amp; she');
// 結果: you & I > he & she
```