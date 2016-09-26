#エイリアスとインポート alias
名前空間を指定してメソッドを呼び出す場合に  
  `\My\Space\Is\Very\Small\echo_message();`  
のように長い名前空間をつけるのは煩雑になるので、エイリアスとインポートを使って記述を短縮することができる。

##エイリアス: aslias
名前空間名を別の名前でも呼び出せるようにする


-- use 元の名前空間 as 別名の名前空間

```php
require_once 'file02.php';
use My\Space\hoge\hage\Anpontan as MySpace;

MySpace\echo_message();
$obj = new MySpace\MySpaceClass();
```

##インポート: import
`use 元の名前空間 + その名前空間に含まれるクラス名 as 別名のクラス名`  
名前空間 My\Space に属する MySpaceClass クラスに直接エイリアスを定義し、且つその別名で My\Space\MySpaceClass を呼び出せる

```php
require_once '20130806_02.php';

use My\Space\MySpaceClass as MyClass;
$obj01 = new MyClass();
```

