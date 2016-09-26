#タプル tuple
配列とも辞書型とも違う  
配列でないので for in が使えない。辞書型ではないので参照に [キー] が使えない。
タプルで要素にアクセスするには **タプル.要素名** のように名前を指定する必要がある

##名前なしタプル

```swift
  // 宣言
  let item = ("ジュース", 100, 0.08, 108)
  
  // 参照は タプル変数名.配列の要素番号 のようにする
  print("商品名=\(item.0), 税抜き価格=\(item.1)円, 消費税=\(item.2 * 100)%, 税込み価格=\(item.3)円")
  // 商品名=ジュース, 税抜き価格=100円, 消費税=8.0%, 税込み価格=108円

```

##複数の変数をまとめて宣言するタプル

```swift
  // 宣言
  let (name, price, tax, priceWithTax) = ("ジュース", 100, 0.08, 108)
  
  // 参照
  print("商品名=\(name), 税抜き価格=\(price)円, 消費税=\(tax * 100)%, 税込み価格=\(priceWithTax)円")
```

##名前つきタプル ★

```swift
  // 宣言
  let item = (name:"ジュース", price:100, tax:0.08, priceWithTax:108)
  
  // 参照
  item.name
  item.price
```

##関数の戻り値としてタプルを返す

```swift
func getTuple() -> (name:String, age:Int) {
  return ("shutaro", 36)
}

let tuple1 = getTuple()
tuple1.name
tuple1.age
```

