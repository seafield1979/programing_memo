#オブジェクト生成 object
クラスの定義を元にオブジェクトを生成する。

**変数宣言**  
クラス変数の宣言方法。普通の変数の型の部分がクラス名になる

```swift
// クラス変数を定義(オブジェクト生成はまだ)
クラス名 変数名;

例:
HogeClass hoge1;
public static HogeClass hoge2;
```

**オブジェクト生成**

```swift
// クラスオブジェクトを生成し、変数に代入
クラス名 変数名 = new クラス名();  

例:
HogeClass hoge1 = new HogeClass();
public static HogeClass hoge2 = HogeClass();
```

