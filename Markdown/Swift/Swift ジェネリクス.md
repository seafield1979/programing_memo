#ジェネリクス generics

C++のテンプレートのようなもの
型が違うだけで処理は同じような場合に、ジェネリックでどの型にも対応する処理を作成できる

```swift
  func bigger<T:Comparable>(val1: T, val2: T) -> T {
    return val1 > val2 ? val1 : val2
  }

  // Intの変数
  var i1 = 10
  var i2 = 20
  print(bigger(i1, val2:i2))
  
  // Floatの変数
  var f1 : Float = 10.0
  var f2 : Float = 20.0
  print(bigger(f1,val2:f2))
  
  var d1 : Double = 10.0
  var d2 : Double = 20.0
  print(bigger(d1, val2:d2))
  
  var s1 : String = "Mickey"
  var s2 : String = "Miffy"
  print(bigger(s1,val2:s2))
```

