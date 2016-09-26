#文字列 string

##基本

```swift
//文字列の結合
var hoge1 = "ほげほげ"
var hoge2 = hoge1 + "はげはげ"    // <- hoge2は"ほげほげはげはげ"
var hoge3 = "いえい"
hoge3 += "あいむはっぴ"           // <- hoge3は"いえいあいむはっぴ"

//変換
数値 -> 文字列
  let str  = num.description // "-1.23"
  let str2 = String(describing:num)
  
//文字列 -> 数値
  let str = "123"
  let intFromStr   = str.toInt()! + 1  // 124
  let intFromOpStr = str.toInt()       // Optional(123)

//フォーマット指定
  let x = 100.123
  let y = 200.123
  let str = String(format: "%.4f %.4f", x, y)

// 文字列の長さ(count)
  str.characters.count

// 分割(split)
// 区切り文字を指定して文字列を分割する
  let str = "hoge,hage,mage"
  let array = str.componentsSeparatedByString(",")
  // array -> ["hoge", "hage", "mage"]

// 結合(join)
// 区切り文字を指定して文字列配列を結合する
  let prefs = ["yamagata", "miyagi", "fukushima"]
  let str = join(',', prefs)
  // str -> "yamagata,miyagi,fukushima"
```