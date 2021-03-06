#定数 const

```sh
//define(識別子, 値);
define("TAX", 0.08);
define(TAX2, 0.10);   // ダブルコーテーションなしでもOK
```

## 自動的に定義される定数

|定数名|説明|
|:--|:--|
|`__LINE__` | ファイル上の現在の行番号。
|`__FILE__` | ファイルのフルパスとファイル名 (シンボリックリンクを解決した後のもの)。 インクルードされるファイルの中で使用された場合、インクルードされるファイルの名前が返されます。
|`__DIR__` | そのファイルの存在するディレクトリ。include の中で使用すると、 インクルードされるファイルの存在するディレクトリを返します。 つまり、これは dirname(`__FILE__`) と同じ意味です。 ルートディレクトリである場合を除き、ディレクトリ名の末尾にスラッシュはつきません。
|`__FUNCTION__` | 関数名。
|`__CLASS__` | クラス名。 クラス名には、そのクラスが宣言されている名前空間も含みます (例 Foo\Bar)。 PHP 5.4 以降では、__CLASS__ がトレイトでも使えることに注意しましょう。トレイトのメソッド内で __CLASS__ を使うと、そのトレイトを use しているクラスの名前を返します。
|`__METHOD__` | クラスのメソッド名。
|`__NAMESPACE__` | 現在の名前空間の名前。