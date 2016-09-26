#辞書型配列 dictionary

宣言の仕方はいくつかの書き方がある。

```swift
  // Dictionary型
  var d0 : Dictionary<String,Int> = ["Apple" : 100, "Orange": 200, "Banana" : 50]

  // 変数の宣言 (nil) キーと値の型を指定できる
  var d1 : [String : Int]? = nil

  // 変数の宣言と初期値設定
  var d12 : [String : Int] = ["Apple" : 100, "Orange": 200, "Banana" : 50]
  
  // 変数の宣言(型を指定しない)
  var d13 : [Strin : Int] = ["Apple" : 100]

```

```swift
  // 代入
  d12["Pine"] = 150

  // 要素の削除
  d12["Apple"] = nil    // nilを代入する

  // キーのリスト取得
  d12.keys.array

  // 値のリスト取得
  d12.values

  // キーでループを回す
  var d7 : [String : Int] = ["Apple" : 100, "Orange" : 150]
  for key in d7.keys {
    print("d7 \(key):\(d7[key])")
  }

  // キーと値をループで回す
  for (key, value) in d7 {
    print("d7 \(key):\(value)")s
  }

  // 要素数を取得 (count)
  d7.count
```

  その他の重要な点  
  
  ・Dictionary型を別の変数に代入すると、値のコピーが行われる（コピー元の値が保持される）