#ネストした型
Swiftではクラスや構造体、列挙型の定義の中に、さらにクラスや構造体、列挙型を定義することができます。型をネストして定義

```swift
  // CNest1クラス内でCNest2を、CNest2クラス内でCNest3クラスを定義
  // CNest1クラス内で CNest2 を、CNest2クラス内でCNest3 を参照できる
  class CNest1 {
      class CNest2 {
          class CNest3 {
              func test1() {
                  print("CNest3:test1")
              }
          }
          var nest3 : CNest3 = CNest3()
          func test1() {
              nest3.test1()
              print("CNest2:test1")
          }
      }
      var nest2 : CNest2 = CNest2()
      func test1() {
          nest2.test1()
          print("CNest1:test1")
      }
  }

  var nest1 : CNest1 = CNest1()
//    var nest2 : CNest2 = CNest2()     これはできない。CNest2が使用できるのはCNest1のスコープ内だけ
```

