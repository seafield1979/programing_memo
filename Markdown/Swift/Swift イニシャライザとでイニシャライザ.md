#イニシャライザ init deinit

  **イニシャライザ** インスタンスが生成されるときに呼ばれる初期化メソッド  
  **デイニシャライザ** インスタンスが破棄される前に呼ばれる後処理メソッド  

###イニシャライザとデイニシャライザ

```swift
class Hoge {
  // イニシャライザ
  init(){
    // 自動で呼ばれるYO
  }

  // パラメータ付きのイニシャライザ
  init(hoge:Int, name:String){
  }

  // デイニシャライザ
  deinit{
    // 解放時に自動で呼ばれYO
  }
}
```

###指定イニシャライザ
指定イニシャライザはすべてのプロパティを初期化するイニシャライザのこと

```swift
class Person{
  var name : String
  var age : Int
  init(name:String, age:Int){
    self.name = name
    self.age = age
  }
}
```

###コンビニエンスイニシャライザ
指定イニシャライザと異なる引数のイニシャライザ。複数定義することも出来る

```swift
class Person {
  var name : String
  var age : Int
  init(name:String) {   // 引数はnameだけ
    self.name = name
    self.age = 0
  }
  init(age:Int){        //　引数はageだけ
    self.name = ""
    self.age = age
  }
}
```