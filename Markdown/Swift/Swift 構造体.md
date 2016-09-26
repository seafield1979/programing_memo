#構造体 struct

主にデータを保持するために特化したクラス。
機能的には継承、デストラクタが使用できないクラス。
イニシャライザはinitをオーバーライドしない場合は全パラメータを引数で受け取るイニシャライザを自動で生成してくれる。

```swift
  // 宣言と参照
  struct Author {
    var name: String = "不明"
    var birthday: NSDate?

    func description() {
      print("name:\(name) birthday:\(birth)")
    }

    // 構造体の値を書き換えるメソッドは先頭にmutatingをつける必要がある
    mutating func flipY() {
      x = -x
    }

  }
  let author = Author()
  let name = author.name
  let birthday : NSDate = author.birthday!
  print(author)  // description()が呼ばれるYO!

```

