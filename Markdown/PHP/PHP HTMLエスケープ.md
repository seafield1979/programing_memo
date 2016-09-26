#HTML escape
htmlで入力された文字をエスケープ。URLで使用不可能な文字列を特殊な文字列に置き換える。

```php
//HTMLエスケープ
//HTML内で特殊な意味を持つ文字を別の文字に置き換える
htmlspecialchars('you & I > he & she');
// 結果: you &amp; I &gt; he &amp; she

//HTMLアンエスケープ
//エスケープしたものを元の文字に戻す
htmlspecialchars_decode('you &amp; I &gt; he &amp; she');
// 結果: you & I > he & she
```