#変数 variable
<!-- variable:: var:: let:: -->
  変数は var で宣言、定数は let で宣言する

##型

|型|説明|
|---|---|
|    Bool|     2値型。trueまたはfalseどちらかの値をとる
|    Int8|    -128〜127 の整数
|    Int16|   -32,768〜32,767 の整数
|    Int32|   -2,147,483,648〜2,147,483,647 の整数
|    Int64|   -9,223,372,036,854,775,808〜9,223,372,036,854,775,807 の整数
|    Int|     32ビット環境ではInt32、64ビット環境ではInt64と同じ範囲の整数値をとる
|    Float|   32ビット浮動小数点。Doubleほどの精度を必要としない場合に使用する
|    Double|  64ビット浮動小数点。小数点を扱う場合は主にこちらを使用する
|    String|  Unicode文字を連ねた文字列を格納する可変長文字列型
|    Character| Unicode文字を１文字格納する文字型
|    Array〈T〉|  Genericsとして定義されているため T の型で縛られる
|    Array〈Any〉|  配列内に複数の型が混在する複合型配列
|    Dictionary〈T, S〉|  T 型のキーと S 型の値を持つ連想配列

##変数宣言

```swift
// var 変数名:型 = 値
var hogehoge:String = "ほげほげ"

// 初期値なしのインスタンス変数はオプショナルにする
var hoge:String?

// 一行に複数の宣言をまとめて書く場合は , でつなげる
var posX : Float, posY : Float

//型を省略すると自動で推測してくれる。
var hoge = "ほげほげ"   // <- OK
var hoge2 = 100        // <- OK

//代入
// 変数 = 値
var hoge:int = 100
hoge = 200

//定数  const value::
// let 変数名:型 = 値
let hoge:String = "ほげ"
hoge = "はげ"         // <- 定数は変更できないエラー発生
```