#文字列表示 print
<!-- print:: -->
  ・printを実行したメソッド名、行番号を出力
      print(__FUNCTION__, __LINE__)
      使用できるリテラルは

    __COLUMN__
    __FILE__
    __FUNCTION__
    __LINE__



```swift
// 改行しない
// terminatorで文字列の終端を指定できる(デフォルトは改行'\n')
print("hoge", terminator: "")

//変数を挿入する  ("" のなかに \(変数名))
let str1:String = "test"
print("str1: \(str1)")

//式をカンマ区切りで渡す
let hoge:String = "syutaro"
print("aaa", "bbb", hoge)

//文字列以外のクラスをdescriptionを使用して出力
let bau : Enemy = Enemy()
print("hello:" + bau.description)

//フォーマット指定
//Stringクラスの format コンストラクタを使用する
let x = 100.123
let y = 200.123
print(String(format: "%.4f %.4f", x, y))
```