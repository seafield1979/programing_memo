#メソッドのオーバーライド
継承先のクラスで継承元のメソッドをオーバーライドする場合は **override**をつける必要がある。

```swift
class Hage : Hoge {
  override init(){}
  override func hogehoge(){    // <- メソッド名の前に"override"をつけるべし
    // 親クラスの機能を呼び出したい場合は
    // super.メソッド名()
  }
}
```

###オーバーライドを禁止する (final)

```swift
class Hoge {
  final hogehoge(){    // <- finalをつけるとこのメソッドはオーバーライド禁止
    ~
  }
}
class Hage : Hoge {
  override func hogehoge(){  // <- オーバーライドできないのでエラーんなる
  }
}
```

