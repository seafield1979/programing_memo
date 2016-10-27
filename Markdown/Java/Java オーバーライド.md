#オーバーライド override
overrideの元々の意味。

~~~
〈命令・権利などを〉無視する，受け付けない; 〈決定などを〉くつがえす，無効にする.
~~~
  
Javaでは親クラスで定義されたメソッドを子クラスでオーバーライド(override)することができる。このとき、処理が変わるのはオーバーライドした子クラスのメソッドのみ。親クラスのメソッドはそのまま。

オーバーライドするには子クラスで親クラスと同名のメソッドを定義するだけで良いが、オーバーライドされたメソッドされたということを強調するために `@Override` をつけることもできる。

```swift
@Override public void hoge();
```

サンプル

```swift
class Hoge {
  public void hoge() {
    System.out.println("hoge");
  }
} 

class HogeSub extends Hoge {
  @Override
  public void hoge() {
    System.out.println("hoge hoge");
  }
}

HogeSub sub1 = HogeSub();
sub1.hoge();    // hoge hoge
```

