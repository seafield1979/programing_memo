##配列の便利関数
Swiftの配列はいろいろな便利関数を使える。

###map

mapは配列の全要素に処理を行い、新しい値を返すような場合に使用する。
例: 

  * 配列の要素を100倍した配列を返す
  * 配列の文字列を大文字に変換した配列を返す

```swift
// 全要素を10倍した新しい配列を取得
let array1 = [1,2,3,4,5]
let newArray = array1.map{ $0 * 10 }
newArray.forEach{ print($0) }  // 10,20,30,40,50
```

###filter
<!-- filter:: -->
filterは配列の要素のうち条件に合致したものを残したい場合に使用する
例: 配列に対して

  * しきい値より大きい値のみ残す
  * 特定の書式の文字列のみ残す

```swift
// 指定の値以上の値のみ残す
let array1 = [1,2,3,4,5,6,7,8,9,10]
let array2 = array1.filter { $0 > 5 }
array2.forEach { print($0) }  // 6,7,8,9,10
```

###sort 
配列の要素の順番を並び替える。
sortはソート後に新しい配列を返す。  
sortInPlaceは元の配列をソート済みに変更する。  

```swift
// sortInPlace
print("-- sortInPlace 1 --")
var arrayIP1 = [50,30,21,35,55,1]
var arrayIP2 = ["hoge", "abc", "123", "ddd", "a123"]

arrayIP1.sortInPlace()
arrayIP1.forEach { print($0) }

arrayIP2.sortInPlace()
arrayIP2.forEach { print($0) }

// 昇順
arrayIP1.sortInPlace(<)
arrayIP1.forEach { print($0) }

// 降順
arrayIP1.sortInPlace(>)
arrayIP1.forEach { print($0) }

// sort
let array1 = [50,30,21,35,55,1]
let array2 = ["hoge", "abc", "123", "ddd", "a123"]

// sortで並べ替え(整数)
array1.sort().forEach { print($0) }

// sortで並べ替え(文字列)
array2.sort().forEach { print($0) }

// 昇順(小さい順)に並び替え
//let newArray = array1.sort {$0 < $1}
let newArray = array1.sort(<)
newArray.forEach {print($0)}

// 降順(大きい順)に並び替え
//let newArray2 = array1.sort {$0 > $1}
let newArray2 = array1.sort(>)
newArray2.forEach {print($0)}

// for文で使用する
for i in array1.sort(>) {
    print(i)
}

```

###reverse 
配列の並びを逆順にする

```swift
let array1 = ["a1", "hoge", "b3", "hage"]
let newArray = array1.reverse()
newArray.forEach { print($0) }

```