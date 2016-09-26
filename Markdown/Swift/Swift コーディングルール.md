#コーディング規約 coding rules
<!-- coding:: -->
[Swiftコーディング規約@Wantedly](http://qiita.com/susieyy/items/f71435cc962e70d81b37)

##命名規則

```swift
~~~swift
// 変数、関数 ハンガリアン記法
let flag = true   // 先頭は小文字
let gameStageFlag = true  // 単語の先頭は大文字

// 関数 
func goHoge() {}
// 第一引数の名前を関数名にふくめる (~FromString)
func goHogeFromString(hoge : String){}
~~~


```

###初期値を代入する場合は変数の型名をつけない
~~~swift
// ◯
let text = "Hoge Fuga"

// ☓
let text: String = "Hoge Fuga"

// 型が曖昧な場合は記述する
// ◯ Int / UInt / NSInteger / NSUInteger の区別がつかないので明記する
let i: Int = 0

// ◯ Float / CGFloat / Double の区別がつかないので明記する
let p: Float = 3.14
~~~

###配列の初期化
第一引数は可読性を考慮して、必要であれば外部引数名を利用する。
~~~swift
// 配列
// ◯
var elements = [Int]()
// ☓
var elements: [Int] = []
~~~

###関数
~~~swift
第一引数は可読性を考慮して、必要であれば外部引数名を利用する。
// ◯
func dateFromString(dateString: String) -> NSDate
dateFromString("2014-03-14") // 引数がStringを取ることが関数から類推できる
~~~