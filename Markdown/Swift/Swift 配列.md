#配列 array
Swiftの配列は要素の型を指定した場合は全要素が同じ型、指定しない場合はいろいろな型の値を混合できる

##配列変数の宣言

```swift
// 宣言
//let array1 : [要素の型]   
letだとimmutable, varだとmutable
var array1 : [Int]    // 要素がInt型の配列 (空)
var array2 : [String] // 要素がString型の配列 (空)
var array3 : [Int] = [1,2,3,4,5]    // 初期化
var array4 : [String] = ["a","b","c","d","e"]   // 初期化
var array5 = [1,"aaa"]    // いろいろな型を混ぜてもOK (Objective-CのNSArrayと同じ)
var array6 : Array<Any> = [2,"bbb"]    // 混合型の型名は Array<Any>
var array7 : [String] = []  // 初期化したけど要素が0の配列
var array72 = [String]()     // 初期化済み0配列  こちらの書き方が推奨
var array8 : [String]    // 未初期化の配列(initで初期化しないとエラーになる)

// 未初期化の配列はinitで初期化する
init() {
  self.array8 = ["a", "b", "c"]
}

//多次元配列
let matrix:[[String]] = [["hoge","fuga"] ,["ohayo","hello"]]
println(matrix[0][1])//"fuga"

// 空の配列を宣言
// 全て文字列の空の配列を宣言
var empty: [String]

// 空の配列は初期化してから使用
empty = Array(count:10, "")
empty = []

// 空の配列に要素を追加していく
var array = [String] = []
array.append("hoge1")
array.append("hoge2")
array.append("hoge3")

// 指定の要素数で初期化 Array:count:repeatedValue
var array = Array(count : [要素数], repeatedValue : [初期値])
```

##配列の参照 (配列名[インデックス])

```swift
// 各要素の参照は 配列名[インデックス]
let array1 : [Int] = [1,2,3,4,5];
print("2:\(array[1]) \(array[2])")    // 2, 3 と出力される

// for ~ inで全要素にアクセス
let array2 = [1,2,3,4,5] 
for i in array2 {
  print(i)
}

// enumerate()を使ってインデックスと値を参照
let array3 = [10,20,30,40,50]
array3.enumerate().forEach { print("index:\($0.0) value:\($0.1)")}

// 配列の内容を表示したい場合は forEach を使うと簡単
let array4 = [1,2,3,4,5]
array4.forEach {print($0)}

for (index, score) in array3.enumerate() {
    print("index:\(index), score: \(score)")
    // index:0, score: 10
    // index:1, score: 20
    // index:2, score: 30
    // index:3, score: 40
    // index:4, score: 50
}

// 混合型を参照する場合
// 混合型の配列から指定の方の値だけを抜き出す
var ks : [AnyObject] = []
var ki : [AnyObject] = []
var array: [AnyObject] = ["abc", 1, "def", 2, "ghi", 3]

for i in ..< array.count {
    // String
    if let string = array[i] as? String {
        ks.append(string)
    }
    // Int
    if let int1 = array[i] as? Int {
        ki.append(int1)
    }
}
print("test3_1: \(ks)")
print("test3_1: \(ki)")
```

## 配列の操作

###要素の追加 (.append)
要素を１件だけ追加する

~~~swift
var array1 : [Int] = [1,2,3,4,5]
array1.append(6)
~~~

###配列の追加 (+=[])
要素をまとめて追加する

~~~swift
var array1 : [Int] = [1,2,3]
array1 += [1,2,3]
~~~

###要素の変更
~~~swift
var array1 : [Int] = [1,2,3]
array1[0] = 10
~~~

###要素の削除 (removeAtIndex)

~~~swift
var array1 : [Int] = [1,2,3]
array1.removeAtIndex(1)
print(array1)     // 2,3
~~~

###repeatedValue 配列を指定の値で初期化する

~~~swift
var array1 : [AnyObject] = Array(count:10, "hoge")
print(array1)      // [hoge,hoge,hoge,hoge,hoge,hoge,hoge,hoge,hoge,hoge]
~~~