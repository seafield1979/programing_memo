#例外処理 exception
<!-- exception:: try:: -->
例外処理の発生とそのキャッチ  
[Swift 2.0 エラー処理入門](http://qiita.com/koishi/items/67cf4d0f51c4d79f1d22)


 * エラーを投げるメソッドが定義できる
 * エラーの種類をenumで定義できる
 * エラーを投げるメソッドを呼び出す場合は先頭に try をつける
 * エラーを投げるメソッドは do ブロックの中で呼び出さなければならない
 * エラーが発生した時にcatchブロックに処理がとぶ。ここでエラーに合わせた適切な処理を行う。

```swift
// エラー定義
throwされるエラーを定義する
enum MyError : ErrorType {
    case Error1
    case Error2
    ...
}

// エラーを投げるメソッド定義
// メソッド定義の最後に throws をつける
func hogeFunc(num : Int) throws{
  if num > 100 {
    // throw で enumで定義したエラーを投げる
    throw MyError.Erro1
  } eles if num < 0 {
    throw MyError.Error2
  }
}

// do~catch
// throws をつけて定義したメソッドを呼び出す場合は、メソッドの先頭に tryをつけて呼び出す

// do ブロックの中でthrowsをつけたメソッドを呼び出す
do {
    // throwsをつけたメソッドを呼び出す場合は try をつける
    try hogeFunc(1)
    try hogeFunc(-1)
    try hogeFunc(101)
}
catch MyError.HogeError1 {
    print("HogeError1 value is too low")
}
catch MyError.HogeError2 {
    print("HogeError2 value is too large")
}
catch {
    print("Unkwnon Error")
}

// do~catch不要の書き方
// throwsをつけたメソッドでも、try? をつけて呼び出せばdo~catchブロックで囲まなくてOK

//try? do~catch が不要になる  エラー発生時、エラーを無視
  try? hogeFunc(1)
//try! do~catch が不要になる  エラー発生時、クラッシュ
  try! hogeFunc(1)


// defer doブロック処理の後に実行したい処理のブロック
do {
    defer {
        // エラーがあろうがなかろうが、最後に必ず実行される処理
        print("Fix defer")
    }
    try hogeFunc(1)
    try hogeFunc(101)
    defer {
        // エラーが起きなかった場合のみ、最後に実行される処理
        print("No error defer")
    }
}
catch {
    print("Error!!")
}
```

