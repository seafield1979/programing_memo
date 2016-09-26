#クラスの継承 inherit
swiftにはNSObjectのような基底クラスはないので、なにも継承しないクラスを作れる

```swift
class Hoge {  // <-NSObjectのようなものは必要ない
  init(){}
  func hogehoge(){
    print("hoge")
  }
}

// 継承するにはclassの右側に継承元のクラスを記述する
class Child1 : Parent1 {
  func test1 {
    self.hoge()   // 親クラスのメソッドを呼び出すことが可能
  }
}
```

