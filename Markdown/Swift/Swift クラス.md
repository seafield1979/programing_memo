#クラス  class
swiftのクラスの特徴

  * init()はイニシャライザといってクラスの初期化時(インスタンス生成時)に呼ばれる。
  * クラス変数は持てない（クラスメソッドは持てる）
  * メンバ変数のゲッター、セッターを定義できる
  * クラスのメソッド内でそのクラスのメンバ変数やメソッドのself.は省略可能

###クラスの宣言 

```swift
class クラス名{
  // プロパティ
  //var プロパティ名 : 型
  //let プロパティ名 : 型
  // ※プロパティ名は通常 _ をつける
  var _name:String = ""
  var _number:Int = 0
  
  //メソッド
  init(name: String, number: Int) {
    _name = name // 引数の変数名とクラスのプロパティを区別するため、selfをつける
    _number = number // 定数もinitの中なら設定できる
  }

  func hello() -> String {
    return "hello, \(name)"
  }
}
```

### オブジェクトの生成

```swift
  // var 変数名 : クラス名 = クラス名(引数名1 : 引数1, 引数名2 : 引数2)
  var user: User = User(name: "taro", number: 100)

  // オブジェクトメソッドの呼び出し
  user.hello()
```

### クラスの判定（あるクラスを継承しているかどうか）

```swift
  // あるクラスそのものか、もしくはを継承しているかを判定するには  as? を使用する
  if let sub1 = object1 as? SubClass {
    // ダウンキャストが成功したらこのブロックが処理される
  }

  // もしくは is演算子を使用する
  if sub1 is SubClass {

  }

  // クラスの判定（特定のクラスかどうか） Objective-cのisMemberOfClass:メソッドと同じ
  if sub1.dynamicType === SubClass.self

  例 hoge.dynamicType == HogeClass.self


  // 同一インスタンスの判定
  var monsterA = Monster()
  monsterA.name = "スライム"
  var monsterB = monsterA

  if monsterA === monsterB {
      print("同じインスタンス")
  }
  if monsterA !== monsterB {
      print("同じインスタンスではない")
  }
```

###インスタンス変数からクラス名を取得 
NSStringFromClass メソッド

```swift
  NSStringFromClass(インスタンス.dynamicType))
  
  var hoge : Hoge = Hoge("hahaha")
  print(NSStringFromClass(インスタンス.dynamicType)))  // ファイル名.Hoge
```

###クラスメソッド
クラスメソッドはオブジェクトなしで呼び出せるメソッド。

```swift
class SomeClass {
    // クラスメソッド
    class func someTypeMethod() {
        // type method implementation goes here
    }
}

// クラスメソッドの呼び出し
// クラス名.クラスメソッド名()
SomeClass.someTypeMethod()
```

