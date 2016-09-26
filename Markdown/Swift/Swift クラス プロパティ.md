#プロパティ Property
<!-- Property:: -->
クラスのプロパティには保持型プロパティ(Stored Property)と計算型プロパティ(Computed Property)の２種類がある。

####保持型プロパティ (Stored Property)
メンバ変数を定義してそれをアクセスする方法。

```swift
class Hoge {
  var name : String
  var age : Int
  init() {
    self.name = "nanashi"
    self.age = 0
  }
}
let hoge : Hoge = Hoge()
// 実態があるのでそのまま参照、変更できる
hoge.name = "Momotaro"
hoge.age = 100
```

####計算型プロパティ (Computed Property)
値を保持せずにゲッターセッターによって値を参照したり変更したりするプロパティ。ただ値を保持しないが計算元として参照する保持型プロパティは必要なので、関連する保持型プロパティは必要。

```swift
class HogeC {
  var _age : Int
  init() {
    self._age = 0
  }
  // 計算型プロパティ
  var age : Int {
    get {
      return self._age
    }
    set(newAge) {
      self._age = newAge
    }
  }
}
let hogeC : HogeC = HogeC()
print(hogeC.age)    // ageのゲッターメソッド(get)が呼ばれる
hogeC.age = 100     // ageのセッターメソッド(set)が呼ばれる
```

