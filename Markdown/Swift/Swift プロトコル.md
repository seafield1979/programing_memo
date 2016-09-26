#プロトコル protocol

Javaのインターフェース、Objective-Cのプロトコルとだいたい同じでメソッドと算出型プロパティのみ定義可能、プロパティは持てない
プロトコルの特徴

  * プロトコルの宣言ではインターフェースの定義のみ。実装は書かない
  * メンバ変数(プロパティ)は持てない
  * １つのクラスに複数のプロトコルを適用することができる

##プロトコルの宣言

```swift
protocol Protocol1 {
  // メソッド
  func hoge()
  func hoge2() -> String

  // 算出型プロパティ get/set を定義すると設定、参照の属性を指定できる
  var str1 : String {get}   // 参照のみ
  var str2 : String {get/set}   // 参照/設定
}
```

##プロトコルの実装
プロトコルを実装するには実装先のクラス宣言時に : の右側にプロトコル名を追加する

```swift
// class クラス名 : プロトコル名
// class クラス名 : 親クラス名, プロトコル名1, プロトコル名2...

class Enemy : Protocol1 {
  var _str1 : String
  var _str2 : String

  // 実装 (プロトコルのメソッドを全部実装する)
  func hoge() { print("hoge") }
  func hoge2() -> String {
    return "hogehoge"
  }
  var str1 : String {
    return self._str1
  }
  var str2 : String {
    get {
      return self._str1
    }
    set(str) {
      self._str2 = str
    }
  }
}
```

##subscript 配列にアクセスするための定義
クラスのオブジェクトに対して [] アクセスされた時の処理を定義できる。見た目が配列っぽいけど機能は異なるため注意。
擬似的に要素番号が連続していない配列などを作る事ができる。

```swift
class TestSubscript
{
    var names : [String]
    init() {
        self.names = []
    }
    
    subscript(index: Int) -> String {
        // hensuu[index] で参照された時の処理 String を返す
        get {
            if index < 0 || index >= names.count {
                return ""
            }
            return names[index]
        }
        // hensuu[index] = name で代入された時の処理
        set(name) {
            if index < 0 || index > names.count {
                return
            }
            names.insert(name, atIndex: index)
        }
    }
}

// 使用
let test1 = TestSubscript()
test1[0] = "111"
test1[0] = "222"
print(test1[0])  // 222
print(test1[1])  // 111
```

