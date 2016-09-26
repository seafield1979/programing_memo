#for文
繰り返し処理を行いたいときに使用する。

```swift
// 基本
//C言語のforループ(非推奨)
for var cnt = 0; cnt < 10; cnt++ {
    print("cnt:\(cnt)")
}

//範囲指定 (tupleを使用)
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
for index in 0 ..< 10 {
    print("\(index)")
}

//高速列挙 (要素数を繰り返す)
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}

//index省略
for _ in 1...3 {
    // 何らかの処理を3回実施します
}

//インデックスを参照する enumerate::
let array = [10,20,30,40,50]
for (index, value) in array.enumerate() {
  print("\(index) : \(value)")
}

//多重ループを一気に抜ける
for_i: for i in 1...5 {
    for j in 1...5 {
        print("i=\(i) j=\(j)")
        if i == 2 && j == 2 {
            print("break for_i")
            break for_i
        }
    }
}
```

###forEach
forEachを使用すると配列のループを簡単に回すことができる

```swift
配列.forEach {
  // 配列の各要素は $0 で参照できる
  // 配列が辞書型[String : Int]等の場合は key=$0.0、value=$0.1
}
```

```swift
// 配列をforループ
let array = [1,2,3,4,5];
array.forEach {
    print($0)
}

// 辞書型をforEachで表示してみる
let dictionary : [String : Int] = ["Apple" : 100, "Orange": 200, "Banana" : 50]
dictionary.forEach {
    print(String(format:"key=%@, value=%d",$0.0, $0.1))
}

```