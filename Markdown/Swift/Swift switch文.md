#switch~case
<!-- switch:: -->
  swiftのswitch文の特徴

  * case に複数の値を設定できる
  　例: case 1,2,3:  
  * breakいらない。
  * breakせずに次のcaseに進みたい場合は fallthrough を使用する
  * 文字列もswitchできる
  

```swift
// 基本
// 整数値でswitch
  switch hoge {
    case 1:
      print("1だよ")
    case 2,3:
      print("2か3だよ")
    case 4...10:
      print("4から10だよ")
    default:
      print("それ以外だよ")
  }

// 文字列でswitch
  switch hoges{
    case "banana":
      print("ばなな")
    case "tomato"
      print("とまと")
    case "cucumber"
      print("きゅうり")
  }

// break、fallthrough
// なにも書かなくてもbreakしてくれるのでbreakは必要ない。
// 次のcaseも処理したい場合はfallthroughする
  switch hoge {
    case 1:
      print("1")
      fallthrough
    case 2:
      print("2")
      break
    case 3:
      print("3")
  }

// 範囲を指定する
  switch (num) {
  csae 0:
    print("zero")
  case 1..<10:
    print("number of 1")
  case 10..<100
    print("number of 2")
  csae 100..<1000
    print("number of 3")
  default:
    print("more than 1000")
  }

// 範囲を指定する (where)
  switch abs(num) {   // absは絶対値を返す関数
  case let n where n == 0:
      print("ゼロ")
  case let n where n < 10:
      print("1桁")
  case let n where n < 100:
      print("2桁")
  case let n where n < 1000:
      print("3桁")
  default:
      print("4桁以上")
  }
```