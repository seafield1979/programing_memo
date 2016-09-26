#関数 function
[Swiftまとめ - 関数 -](http://qiita.com/merrill/items/2e143c0e3ecfb04243a2)

###基本

```swift
// 関数の宣言
func 関数名(引数名1 : 引数型1, 引数名2 : 引数型2) -> 戻り値型 {
}
```

  Swiftの関数の特徴

  * 引数に名前が設定できる
  * 引数に初期値を設定できる
  * 引数の名前が異なれば、同じ名前のメソッドを宣言できる
  * 戻り値を複数返せる（タプル（リスト））
  * 外部引数を使用してObjective-Cみたいに呼び出し側でパラメータの名前を指定できる

###関数のサンプル

```swift
//①引数なし・戻り値なし
func hello(){
  print("Hello!")
}
```

```swift
//②引数なし・戻り値あり
func hoge() -> Int{
  return 100
}
```

```swift
//③引数あり・戻り値あり
func hoge(num: Int) -> Int{
  return num
}

hoge(100)
```

```swift
//④引数あり（複数）
func hoge(a: Int, b: String){
  print(a)
  print(b)
}

hoge(100, "テスト")
```

```swift
//⑤引数あり（複数かつ数を指定しない）
//引数の値は配列として格納される。
func hoge(a: Int...){
  print(a[0])
  print(a[1])
}
hoge(100, 200)
```

```swift
//⑥引数あり（初期値を設定）
func hoge(a: Int = 100){
  print(a)
}

hoge() // => 100
hoge(a: 200) // => 200 ※引数にデフォルト値を指定した場合は、引数のラベル名(ライブ引数名)が必要になる！
```

```swift
//⑥引数を使って変数宣言
//引数のパラメータは、通常定数(let)として扱われるので、値を代入することはできない。
func hoge(a: Int){
  a = 200 // エラー
}
hoge(100)

//ただし、引数でinout宣言をしてあげるとエラーにならない。
func hoge(inout a: Int){
  a = 200 // OK
}
hoge(100)
```

```swift
//⑦キーワード引数（外部引数）を使う
//引数の変数名の前に#をつける。
func hoge(#a: Int, #b: Int){
  print(format: "%d %d", a, b)
}
hoge(a: 100, b:200)

//あまりないと思うが、引数名とは別名で指定したい場合は、以下のように書ける(外部引数名)。
//func 関数名(外部引数名 内部引数名 : 変数型)
func hoge(num a: Int){
 print(a)
}
hoge(num: 100)
```

```swift
//⑧戻り値が複数
//タプルを使用する。
func compareNumber(arr: [Int]) -> (min: Int, max: Int) {
    var min = minElement(arr)
    var max = maxElement(arr)

    return (min, max)
}

var result = compareNumber([1,2,3,4,5,6,7,8,9,10])
result.min // => 1
result.max // => 10
```

###オーバーロード
引数の数や、外部引数の名前が異なれば、同じ名前の関数をたくさん定義できる。

```swift
func hoge(){
  print(100)
}
func hoge(#a: Int){
  print(a)
}
func hoge(#b: Int){
  print(b)
}
hoge() // => 100
hoge(a: 200) // => 200
hoge(b: 300) // => 300
```

###可変引数。可変引数は配列として関数に渡される

```swift
func containsCharacter(text:String, characters:Character ...) {
  for ch in characters {
      for t in text {
          if ch == t {
              return true
          }
      }
  }
  return false
}
containsCharacter("abcdefg", characters:"i") // false
containsCharacter("abcdefg", characters:"g", "h", "i") // true
```