#列挙方 enum

swiftのenumは高機能。
swiftのenumの特徴

* 文字列が使用できる（型が指定可能)
* 値を設定できる
* メソッドを指定できる（列挙型の値によって呼び出すメソッドを指定できる)
* 整数値から列挙型に変換できる
  

##基本
~~~swift
enum Signal {
    case Blue       // 青
    case Yellow     // 黄
    case Red        // 赤
}
    
// １行に書く場合
enum Signal {
    case Blue, Yellow, Red
}
~~~

## 型を指定
Swiftのenumは型を指定できる。他の言語のenumは整数値しかもてないが
~~~swift
  // 整数
  enum Signal : Int {
      case Blue       // 青
      case Yellow     // 黄
      case Red        // 赤
  }

  enum Signal : Int {
      case Blue=1
      case Yellow=2
      case Red=3
  }

  // 文字列を返す
  enum SignalStr : String {
      case Blue = "青"
      case Yellow = "黄"
      case Red = "赤"
  }

  // 整数型enum変数に整数値を代入する方法
  let signal = Signal.init(rawValue:1)!

~~~

##メソッドを定義
enum内に関数を定義して機能を持たせることができる

~~~swift
  enum Signal {
      case Blue
      case Yellow
      case Red
      func meaning() -> String {
          switch(self) {
          case .Blue:
              return "進め"
          case .Yellow:
              return "注意"
          case .Red:
              return "止れ"
          }
      }
  }

  // enum型の変数に.をつけて呼び出す
  let s = SignalF.Yellow
  print("meaning:" + s.meaning())
~~~

##参照
~~~swift
  // enum型.値
  let signal1 = Signal.Blue
  let signal2 = Signal.Red

  // 関数の呼び出し
  let s = Signal.Yello
  print(s.meaning)

  // 関連値
  // あまり使わなさそうなのでスキップ

  // switch文
  let s = SignalF.Yellow
  switch s {
  case .Blue:
    print("青")
  case .Yellow:
    print("黄")
  case .Red:
    print("赤")
  }
~~~

## 整数値とenum型の相互変換

```swift
// 整数値からenum型
  let sig : Signal = Signal.init(rawValue:1)!
  
// enum型から整数値
  let sigVal : Int = sig3.rawValue
```