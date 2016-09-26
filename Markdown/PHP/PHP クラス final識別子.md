#クラス finalキーワード
final メソッドは継承先のクラスで上書きできない。  
final クラスは継承できない。  

```php
class Hoge {
  final public function hoge(){
    // 子クラスで上書き不可
  }
}

final class CHoge{
  // 拡張（継承）不可  
}
```