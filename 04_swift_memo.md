<!-- Stylesheet -->
<link href="http://kevinburke.bitbucket.org/markdowncss/markdown.css" rel="stylesheet"></link>
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">Swift Memo</span>

<!-- もくじ -->
[TOC]

Swiftも基本的なプログラムの構造はCocoaライブラリを使用しているのでObjective-Cと大きく変わらない。
  

#学習webページ
  [Webブラウザ上でSwiftのプログラムが確認できる](http://qiita.com/HirofumiYashima/items/03bf6b76d7fa9269c6cd)

#基本仕様

  * 文の最後に;をつける必要がない。
  * メソッドの宣言がない代わりに呼び出しより先にメソッドの本体を置く。
  * 自分で作成したプロジェクト内のswiftクラスはimportの記述は必要ない
  * ヘッダーは必要ない。クラスの定義(Objective-Cのinterface)も必要ない
  * プロパティは基本strong, weakにしたかったら weakを付ける

#Swiftコーディング規約
<!-- coding:: -->
[Swiftコーディング規約@Wantedly](http://qiita.com/susieyy/items/f71435cc962e70d81b37)

##命名規則
<!-- naming -->
~~~swift
// 変数、関数 ハンガリアン記法
let flag = true   // 先頭は小文字
let gameStageFlag = true  // 単語の先頭は大文字

// 関数 
func goHoge() {}
// 第一引数の名前を関数名にふくめる (~FromString)
func goHogeFromString(hoge : String){}
~~~

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

#文字列表示 (print)
<!-- print:: -->
  ・printを実行したメソッド名、行番号を出力
      print(__FUNCTION__, __LINE__)
      使用できるリテラルは

    __COLUMN__
    __FILE__
    __FUNCTION__
    __LINE__

~~~swift
// 改行しない
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
~~~

#基本
<!-- basis::  -->
##コメント
<!-- comment: -->
```swift
//一行コメント
/* 
  複数行コメント
 */
```

  * 行の終わりに;(セミコロン)は不要
  * ヘッダーのインポートは不要（というかヘッダーファイルがない）
      ただし、swiftのコードをobjective-cに取り込む場合はimportが必要
        http://qiita.com/a_yasui/items/816abdb8acedfcc647fc 
  * pragma mark の記述方法が変わった<br>
      // MARK: <-ここに説明
  * プリプロセッサは #if #elseif #endif しか使えない

#変数
<!-- variable:: var:: let:: -->
  変数は var で宣言、定数は let で宣言する

##型
    Bool     2値型。trueまたはfalseどちらかの値をとる
    Int8    -128〜127 の整数
    Int16   -32,768〜32,767 の整数
    Int32   -2,147,483,648〜2,147,483,647 の整数
    Int64   -9,223,372,036,854,775,808〜9,223,372,036,854,775,807 の整数
    Int     32ビット環境ではInt32、64ビット環境ではInt64と同じ範囲の整数値をとる
    Float   32ビット浮動小数点。Doubleほどの精度を必要としない場合に使用する
    Double  64ビット浮動小数点。小数点を扱う場合は主にこちらを使用する
    String  Unicode文字を連ねた文字列を格納する可変長文字列型
    Character Unicode文字を１文字格納する文字型
    Array〈T〉  Genericsとして定義されているため T の型で縛られる
    Array〈Any〉  配列内に複数の型が混在する複合型配列
    Dictionary〈T, S〉  T 型のキーと S 型の値を持つ連想配列

##変数宣言
<!-- variable declaration:: -->
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

#if文
<!-- if::  -->
  Objective-Cとほぼ一緒。違うのは条件文を()で囲まない、
  処理の部分は必ず{} で囲む必要がある

```swift
  if a == b {
    // 条件が成立した時の処理
  } else if a < b {
    // 最初の条件が成立せずに2つめの条件が成立した時の処理
  } else {
    // 条件が成立しなかった時の処理
  }
```

    条件式
      swiftでは代入式は値を返さないので 
        if a = b {
        }
      はエラー( type '()' does not conform to protocol 'Boolean Type') になる。
      if文の条件式には必ずBoolean型の値を与えてやらないとダメみたい。
      以下もだめ
      var a:Int = 1
      if a {

      }

#for文
繰り返し処理を行いたいときに使用する。

<!-- for:: -->
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


#switch~case
<!-- switch:: -->
  swiftのswitch文の特徴

  * case に複数の値を設定できる<br>
  　例: case 1,2,3:  
  * breakいらない。
  * breakせずに次のcaseに進みたい場合は fallthrough を使用する
  * 文字列もswitchできる
    
```swift
// 基本
  switch hoge {
    case 1:
      print("1だよ")
    case 2,3:
      print("2か3だよ")
    case 4...10:
      print("4から10だよ")
    default:
      print("それ以外だよ")
  }

// 文字列
  switch hoges{
    case "banana":
      print("ばなな")
    case "tomato"
      print("とまと")
    case "cucumber"
      print("きゅうり")
  }

// break、fallthrough
  switch hoge {
    case 1:
      print("1")
      fallthrough
    case 2:
      print("2")
      break
    case 3:
      print("3")
  }

// 範囲を指定する
  switch (num) {
  csae 0:
    print("zero")
  case 1..<10:
    print("number of 1")
  case 10..<100
    print("number of 2")
  csae 100..<1000
    print("number of 3")
  default:
    print("more than 1000")
  }

// 範囲を指定する (where)
  switch abs(num) {   // absは絶対値を返す関数
  case let n where n == 0:
      print("ゼロ")
  case let n where n < 10:
      print("1桁")
  case let n where n < 100:
      print("2桁")
  case let n where n < 1000:
      print("3桁")
  default:
      print("4桁以上")
  }
```

#文字列
<!-- string:: str::-->

##基本
~~~swift
//文字列の結合
var hoge1 = "ほげほげ"
var hoge2 = hoge1 + "はげはげ"    // <- hoge2は"ほげほげはげはげ"
var hoge3 = "いえい"
hoge3 += "あいむはっぴ"           // <- hoge3は"いえいあいむはっぴ"

//変換
数値 -> 文字列
  let str  = num.description // "-1.23"
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
// 区切り文字を指定して文字列の配列を結合する
  let prefs = ["yamagata", "miyagi", "fukushima"]
  let str = join(',', prefs)
  // str -> "yamagata,miyagi,fukushima"

~~~

##正規表現処理クラス
正規表現でマッチているかチェック、マッチした文字列を取得するには以下のクラスを使うと便利。
~~~swift
class Regexp {
    let internalRegexp: NSRegularExpression
    let pattern: String
    
    init(_ pattern: String) {
        self.pattern = pattern
        do {
            self.internalRegexp = try NSRegularExpression(pattern: pattern, options: [.CaseInsensitive])
        } catch let error as NSError {
            print(error.localizedDescription)
            self.internalRegexp = NSRegularExpression()
        }
    }
    
    func isMatch(input: String) -> Bool {
        let matches = self.internalRegexp.matchesInString( input, options: [], range:NSMakeRange(0, input.characters.count) )
        return matches.count > 0
    }

    func matches(input: String) -> [String]? {
        let nsInput = input as NSString
        if self.isMatch(input) {
            var results = [String]()
            if let matches = internalRegexp.firstMatchInString(nsInput as String, options: NSMatchingOptions(), range: NSMakeRange(0, input.characters.count))
            {
                for i in 0...matches.numberOfRanges - 1 {
                    results.append(nsInput.substringWithRange(matches.rangeAtIndex(i)))
                }
            }
            return results
        }
        return nil
    }
}
~~~

#is と as
###as
<!-- is -->
型チェック  
インスタンスに対してチェックを行い、True/Falseを返す  
`[インスタンス] is [チェックしたいクラス]`

~~~swift
let array : [Any] = ["text", 10, 20, "30"]
for obj in array {
    if obj is Int {
        print("\(obj) is Int")
    }
    else if obj is String{
        print("\(obj) is String")
    }
}
~~~

###is
<!-- as -->

 * インスタンスをダウンキャストする
 * 数字=>文字列のような変換をするものではない
 * 共通するベースクラスにアップキャストされたインスタンスをダウンキャストする場合に使用する
 * as?とすると、変換が失敗した場合はnilを返すようにできる
 * つけないと、失敗した場合ランタイムエラーになる

~~~swift
// 強制的にダウンキャストする。失敗した場合はランタイムエラー
let [ダウンキャストされたインスタンス] = [インスタンス] as [ダウンキャスト先のクラス]
// ダウンキャスを試みて、失敗した場合はnilを返す
let [ダウンキャストされたインスタンス(失敗すればnilが入る)] = [インスタンス] as? [ダウンキャスト先のクラス]

// Animalクラスとそれを継承したDog/Catクラスを用意する
class Animal{

}
class Cat: Animal{
    let say = "nya-"
}
class Dog: Animal{
    let say = "wan wan"
}

// animalsの型はDogとCatの共通ベースクラスにアップキャストされたAnimal配列になる
// let animals = [Dog(), Cat()] でも型推論で同じ動作になる
let animals : Animal[] = [Dog(), Cat()]

// animalは配列animalsの要素なので、型はAnimalとして扱われる
for animal in animals {
    // Animal型にアップキャストされているanimalをCatでダウンキャストするように試みる
    if let cat = animal as? Cat {
        // ダウンキャストされてCatのインスタンスとして扱えるので、sayプロパティが使える
        println("Cat say \(cat.say)")

    // 同様にDogにダウンキャストを試みる
    } else if let dog = animal as? Dog {
        println("Dog say \(dog.say)")
    }
}


~~~



#配列 array
<!-- array:: -->
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

    var array1 : [Int] = [1,2,3,4,5]
    array1.append(6)
###配列の追加 (+=[])
要素をまとめて追加する

    var array1 : [Int] = [1,2,3]
    array1 += [1,2,3]

###要素の変更
    var array1 : [Int] = [1,2,3]
    array1[0] = 10

###要素の削除 (removeAtIndex)
    var array1 : [Int] = [1,2,3]
    array1.removeAtIndex(1)
    print(array1)     // 2,3

###repeatedValue 配列を指定の値で初期化する
    var array1 : [AnyObject] = Array(count:10, "hoge")
    print(array1)      // [hoge,hoge,hoge,hoge,hoge,hoge,hoge,hoge,hoge,hoge]

##便利関数
###map
<!-- map:: -->

mapは配列の全要素に処理を行い、新しい値を返すような場合に使用する。
例: 

  * 配列の要素を100倍した配列を返す
  * 配列の文字列を大文字に変換した配列を返す

~~~swift
// 全要素を10倍した新しい配列を取得
let array1 = [1,2,3,4,5]
let newArray = array1.map{ $0 * 10 }
newArray.forEach{ print($0) }  // 10,20,30,40,50
~~~

###filter
<!-- filter:: -->
filterは配列の要素のうち条件に合致したものを残したい場合に使用する
例: 配列に対して

  * しきい値より大きい値のみ残す
  * 特定の書式の文字列のみ残す

~~~swift
// 指定の値以上の値のみ残す
let array1 = [1,2,3,4,5,6,7,8,9,10]
let array2 = array1.filter { $0 > 5 }
array2.forEach { print($0) }  // 6,7,8,9,10
~~~

###sort 
<!-- sort:: -->
配列の要素の順番を並び替える。
sortはソート後に新しい配列を返す。  
sortInPlaceは元の配列をソート済みに変更する。  

~~~swift
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
~~~

###reverse 
配列の並びを逆順にする
~~~swift
let array1 = ["a1", "hoge", "b3", "hage"]
let newArray = array1.reverse()
newArray.forEach { print($0) }
~~~

#辞書型配列 dictionary
<!-- dictionary:: -->
```swift
  // 変数の宣言 (nil) キーと値の型を指定できる
  var d1 : [String : Int]? = nil

  // 変数の宣言と初期値設定
  var d12 : [String : Int] = ["Apple" : 100, "Orange": 200, "Banana" : 50]
  
  // 変数の宣言(型を指定しない)
  var d13 : [Strin : Int] = ["Apple" : 100]

  // 代入
  d12["Pine"] = 150

  // 要素の削除
  d12["Apple"] = nil    // nilを代入する

  // キーのリスト取得
  d12.keys.array

  // 値のリスト取得
  d12.values

  // キーでループを回す
  var d7 : [String : Int] = ["Apple" : 100, "Orange" : 150]
  for key in d7.keys {
    print("d7 \(key):\(d7[key])")
  }

  // キーと値をループで回す
  for (key, value) in d7 {
    print("d7 \(key):\(value)")s
  }

  // 要素数を取得 (count)
  d7.count
```

  その他の重要な点  
  ・Dictionary型を別の変数に代入すると、値のコピーが行われる（コピー元の値が保持される）


#タプル tuple
<!-- tople:: -->
  配列とも辞書型とも違う  
  配列でないので for in が使えない。辞書型ではないので参照に [キー] が使えない。
  タプルで要素にアクセスするには **タプル.要素名** のように名前を指定する必要がある

###名前なしタプル
    // 宣言
    let item = ("ジュース", 100, 0.08, 108)

    // 参照は タプル変数名.配列の要素番号 のようにする
    print("商品名=\(item.0), 税抜き価格=\(item.1)円, 消費税=\(item.2 * 100)%, 税込み価格=\(item.3)円")
    // 商品名=ジュース, 税抜き価格=100円, 消費税=8.0%, 税込み価格=108円

###複数の変数をまとめて宣言するタプル
    // 宣言
    let (name, price, tax, priceWithTax) = ("ジュース", 100, 0.08, 108)

    // 参照
    print("商品名=\(name), 税抜き価格=\(price)円, 消費税=\(tax * 100)%, 税込み価格=\(priceWithTax)円")

###名前つきタプル ★
    // 宣言
    let item = (name:"ジュース", price:100, tax:0.08, priceWithTax:108)

    // 参照
    item.name
    item.price
    
###関数の戻り値としてタプルを返す
    func getTuple() -> (name:String, age:Int) {
      return ("shutaro", 36)
    }

    let tuple1 = getTuple()
    tuple1.name
    tuple1.age

#関数 function
<!-- function:: func:: -->
    (Swiftまとめ - 関数 -)[http://qiita.com/merrill/items/2e143c0e3ecfb04243a2]
###基本
    // 関数の宣言
    func 関数名(引数名1 : 引数型1, 引数名2 : 引数型2) -> 戻り値型 {
    }

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

//②引数なし・戻り値あり
func hoge() -> Int{
  return 100
}

//③引数あり・戻り値あり
func hoge(num: Int) -> Int{
  return num
}

hoge(100)

//④引数あり（複数）
func hoge(a: Int, b: String){
  print(a)
  print(b)
}

hoge(100, "テスト")

//⑤引数あり（複数かつ数を指定しない）
//引数の値は配列として格納される。
func hoge(a: Int...){
  print(a[0])
  print(a[1])
}
hoge(100, 200)

//⑥引数あり（初期値を設定）
func hoge(a: Int = 100){
  print(a)
}

hoge() // => 100
hoge(a: 200) // => 200 ※引数にデフォルト値を指定した場合は、引数のラベル名(ライブ引数名)が必要になる！

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

#クラス  class
<!-- class:: -->

swiftのクラスの特徴

  * init()はイニシャライザといってクラスの初期化時(インスタンス生成時)に呼ばれる。
  * クラス変数は持てない（クラスメソッドは持てる）
  * メンバ変数のゲッター、セッターを定義できる
  * クラスのメソッド内でそのクラスのメンバ変数やメソッドのself.は省略可能

###クラスの宣言 

~~~swift
class クラス名{
  // プロパティ
  //var プロパティ名 : 型
  //let プロパティ名 : 型
  // ※プロパティ名は通常 _ をつける
  var _name:String = ""
  var _number:Int = 0
  
  //メソッド
  init(name: String, number: Int) {
    _name = name // 引数の変数名とクラスのプロパティを区別するため、selfをつける
    _number = number // 定数もinitの中なら設定できる
  }

  func hello() -> String {
    return "hello, \(name)"
  }
}
~~~

### オブジェクトの生成
~~~swift
  // var 変数名 : クラス名 = クラス名(引数名1 : 引数1, 引数名2 : 引数2)
  var user: User = User(name: "taro", number: 100)

  // オブジェクトメソッドの呼び出し
  user.hello()
~~~

### クラスの判定（あるクラスを継承しているかどうか）
~~~swift
  // あるクラスを継承しているかを判定するには  as? を使用する
  if let sub1 = object1 as? SubClass {
  }

  // もしくは is演算子を使用する
  if sub1 is SubClass {

  }


  // クラスの判定（特定のクラスかどうか） Objective-cのisMemberOfClass:メソッドと同じ
  if sub1.dynamicType === SubClass.self


  // 同一インスタンスの判定
  var monsterA = Monster()
  monsterA.name = "スライム"
  var monsterB = monsterA

  if monsterA === monsterB {
      print("同じインスタンス")
  }
  if monsterA !== monsterB {
      print("同じインスタンスではない")
  }
~~~

###インスタンス変数からクラス名を取得
```swift
NSStringFromClass(インスタンス.dynamicType))

var hoge : Hoge = Hoge("hahaha")
print(NSStringFromClass(インスタンス.dynamicType)))  // ファイル名.Hoge
```

###クラスメソッド
クラスメソッドはオブジェクトなしで呼び出せるメソッド。

~~~swift
class SomeClass {
    // クラスメソッド
    class func someTypeMethod() {
        // type method implementation goes here
    }
}

// クラスメソッドの呼び出し
// クラス名.クラスメソッド名()
SomeClass.someTypeMethod()
~~~

###クラスの継承 inherit
<!-- inherit:: -->
swiftにはNSObjectのような基底クラスはないので、なにも継承しないクラスを作れる

~~~swift
class Hoge {  // <-NSObjectのようなものは必要ない
  init(){}
  func hogehoge(){
    print("hoge")
  }
}

// 継承するにはclassの右側に継承元のクラスを記述する
class Child1 : Parent1 {
  func test1 {
    self.hoge()   // 親クラスのメソッドを呼び出すことが可能
  }
}
~~~

###メソッドのオーバーライド
<!-- override:: -->
継承先のクラスで継承元のメソッドをオーバーライドする場合は **override** をつける必要がある。

~~~swift
class Hage : Hoge {
  override init(){}
  override func hogehoge(){    // <- メソッド名の前に"override"をつけるべし
    
  }
}
~~~

###オーバーライドを禁止する (final)
<!-- final:: -->
~~~swift
class Hoge {
  final hogehoge(){    // <- finalをつけるとこのメソッドはオーバーライド禁止
    ~
  }
}
class Hage : Hoge {
  override func hogehoge(){  // <- オーバーライドできないのでエラーんなる
  }
}
~~~

###イニシャライザ init deinit
<!-- init:: deinit:: -->
  **イニシャライザ** インスタンスが生成されるときに呼ばれる初期化メソッド  
  **デイニシャライザ** インスタンスが破棄される前に呼ばれる後処理メソッド  

####イニシャライザとデイニシャライザ
~~~swift
class Hoge {
  // イニシャライザ
  init(){
    // 自動で呼ばれるYO
  }

  // パラメータ付きのイニシャライザ
  init(hoge:Int, name:String){
  }

  // デイニシャライザ
  deinit{
    // 解放時に自動で呼ばれYO
  }
}
~~~

####指定イニシャライザ
指定イニシャライザはすべてのプロパティを初期化するイニシャライザのこと
~~~swift
class Person{
  var name : String
  var age : Int
  init(name:String, age:Int){
    self.name = name
    self.age = age
  }
}
~~~

####コンビニエンスイニシャライザ
指定イニシャライザと異なる引数のイニシャライザ。複数定義することも出来る
~~~swift
class Person {
  var name : String
  var age : Int
  init(name:String) {   // 引数はnameだけ
    self.name = name
    self.age = 0
  }
  init(age:Int){        //　引数はageだけ
    self.name = ""
    self.age = age
  }
}
~~~

###プロパティ (Property)
<!-- Property:: -->
クラスのプロパティには保持型プロパティ(Stored Property)と計算型プロパティ(Computed Property)の２種類がある。

####保持型プロパティ (Stored Property)
メンバ変数を定義してそれをアクセスする方法。
~~~swift
class Hoge {
  var name : String
  var age : Int
  init() {
    self.name = "nanashi"
    self.age = 0
  }
}
let hoge : Hoge = Hoge()
// 実態があるのでそのまま参照、変更できる
hoge.name = "Momotaro"
hoge.age = 100
~~~

####計算型プロパティ (Computed Property)
値を保持せずにゲッターセッターによって値を参照したり変更したりするプロパティ。ただ値を保持しないが計算元として参照する保持型プロパティは必要なので、関連する保持型プロパティは必要。

~~~swift
class HogeC {
  var _age : Int
  init() {
    self._age = 0
  }
  // 計算型プロパティ
  var age : Int {
    get {
      return self._age
    }
    set(newAge) {
      self._age = newAge
    }
  }
}
let hogeC : HogeC = HogeC()
print(hogeC.age)    // ageのゲッターメソッド(get)が呼ばれる
hogeC.age = 100     // ageのセッターメソッド(set)が呼ばれる
~~~

####ゲッターセッター  getter/setter

クラスの外部からプロパティを参照、代入された時に呼ばれるメソッドをゲッター、セッターとして定義することができる。  
**ゲッター** 参照時に呼ばれるメソッド  
**セッター** 代入時に呼ばれるメソッド  
Objective-Cと異なり、メンバ変数と直接関連はないので自由に命名できる

~~~swift
  // メンバ変数は外から呼ばれるプロパティ名と区別しやすいように先頭に_をつける
  var _hoge : String = ""
  // 
  var hoge:String {
    get {           // ゲッター
        return _hoge
    }
    set (hoge) {    // セッター
      _hoge = hoge
    }
  }
~~~
  ※ _hogeとhogeは直接的な関連はないので全く異なる名前でもOK。

   ゲッターだけの場合はスコープを省略できる
~~~swift
    var age : Int {
      return 100
    }
~~~

###値変更の監視
値が変更される前と後で指定のメソッドが呼ばれる。ただし、イニシャライザ内で変更される場合は呼ばれない。
~~~swift
var age : Int = 0{
  // 設定前に呼ばれる
  willSet {
    // 新しい値は newValue で参照できる
    print("willSet \(self.age) -> \(newValue)")
  }
  // 設定後に呼ばれる
  didSet {
    // 古い値は oldvalue で参照できる
    print("didSet \(oldValue) -> \(self.age)")
  }
}
~~~

###ゲッターセッターと値の監視を共存
~~~swift

// ここでプロパティの定義
var _price : Int = 0{
  willSet {

  }
  didSet {

  }
}
var price : Int {
  get {
    return _price
  }
  set(value) {
    _price = value
  }
}

~~~

###クラスのキャスト cast
<!-- cast:: -->
swiftのキャストは 

  * 子クラスのインスタンスを親クラスにキャスト -> OK
  * 親クラスのインスタンスを子クラスにキャスト -> NG
  * 関係のないクラスにキャスト -> NG

  キャスト時の動作
  子クラスを親クラスにキャストした場合、キャストされた親クラス経由で子クラスでオーバーライドされたメソッドを呼んだ場合、
  子クラスのメソッドが呼ばれる。
  
~~~swift  
  class P1 {
    func goBack(){ print("P1:goBack") }
  }
  class C1 : P1{
    override goBack(){ print("C1:goBack") }
  }
  var obj1 : P1 = C1()  // 子供のインスタンスを親クラスにキャスト
  obj1.goBack()     // C1:goBack  obj1の実態はP1のインターフェイスに限定されたP1のインスタンス
~~~


###エクステンション extension
<!-- extension:: -->
エクステンションは既存のクラス(標準クラスを含む)を拡張するための機能
エクステンションの特徴
  * 計算型プロパティと静的な計算型プロパティ（計算型の型プロパティ）を追加できる
  * インスタンスメソッドと型メソッドを追加できる
  * イニシャライザを追加できます。（追加できるのはコンビニエンスイニシャライザのみ）
  * サブスクリプトを追加できる
  * ネストした型を定義してそれを使うことができる
  * 新たなプロトコルに適合させることができる

  エクステンションのファイル名は [拡張元のクラス]+[機能名].swift  
    例: String+Regext.swift 
  エクステンションを使う場合は、次の様に型名の前にextensionキーワードをつけます。

~~~swift
extension 拡張元のクラス名 {
    // 新たなメソッドの追加やプロトコルへの適応
}
~~~

エクステンションにより新たなプロトコルへ適応させる場合は、次の様に記述します。
~~~swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // プロトコルへ適応するためのメソッド
}
~~~

####拡張例
~~~swift
extension Int {
    // 16進数
    var hex: String {
        return String(self, radix: 16)
    }
    // 8進数
    var oct: String {
        return String(self, radix: 8)
    }
    // 2進数
    var bin: String {
        return String(self, radix: 2)
    }
    // メソッド
    func multi100() -> Int {
        return self * 100
    }
}

// 呼び出し
print("hex:" + 256.hex)
print("oct:" + 256.oct)
print("bin:" + 256.bin)
print("hoge:" + 256.hoge().description)

~~~

#pragma
<!-- pragma:: -->
`pragma - mark` は swfitでは
~~~swift
// MARK: - ここに説明 
※MAKR と : の間にスペースを入れないこと
~~~
のように記述する

#オプショナル optional
<!-- optional -->
  [\[Swift\] Optional 型についてのまとめ Ver2](http://qiita.com/cotrpepe/items/518c4476ca957a42f5f1)

  通常の変数はnilを代入できない。nilを代入できるようにしたのがOptional型
###オプショナル型の書式
~~~swift
//Int 型
  var a: Int? // Optional 型
  var b: Int  // 非 optional 型
//String 型
  var a: String? // Optional 型
  var b: String  // 非 optional 型

  //別の書き方
  T? = Optional<T>
  var a: Int?
  var b: Optional<Int> // Int?　と同じ意味

  Optional 型の初期値は nil
  var a: Int? // Optional 型
  print(a)  // -> nil

  Optional 型は、非 Optional 型と同じようには扱えない
    var b: Int? = 1
    print(b + 2)    // error bはInt? で 2はIntなので型の異なるクラスの演算ができない
~~~

###unwrapする (その1 Force Unwrap)
optional型の変数を使用する場合は hoge!  のように変数名の後ろに!をつけてアンラップし使用できるようにする。
~~~swift
    var a: String? = "hoge"
    print(a!)
~~~
###unwrapする（その２ Optional Chaining） 
!でアンラップすると変数がnilのときに実行時エラーが発生する。  
?でアンラップすると変数がnilのときにエラーが発生せずにnilが返る  
  →アンラップしたときにnilが受け取れない可能性がある場所で使用するとエラーになる。  
~~~swift
  var hoge : String? = nil
  print(hoge?)     // これはエラー。printの引数はnilを受け取れない
  var hoge2 : String? = hoge?.uppercaseStr  // これはOK
~~~

###unwrapする (その３)
?? でアンラップするとnilのときに返す値を別途設定できる
~~~swift
    var str : String?
    print(str ?? "nilですね")    // strがnilのときに"nilですね"が表示される
~~~

###nil判定 if let 
オプショナル型の値がnilかどうかで処理を分岐したい場合の書式。単純にif文でnilチェックをする事もできるが、Swiftでは if文の中で letに代入した結果の判定でnilチェックする方法が用意されている。

~~~swift
var hoge : Hoge? = nil
1. if hoge != nil {}            // そのまま
2. if let hogehoge = hoge {    // hogeがnil以外なら真
      print(hogehoge)     // 真のブロックで hogehoge が使用できる
   } else {
      print("hogeはnilだ");
   }
~~~

#console::
コンソールプログラム。
##swiftのコンソールプログラム作成
  * メニューの [File] - [New] - [Project]<br>
    もしくは Cmd + Shift + N  
  * 左メニューの [OS X] - Application を選択  
  * 右リストの Command Line Tool を選択  
  * 右下の [Next] ボタンを押す  

  ![プロジェクト作成1](http://sunsunsoft.com/image/memo/swift_project.png)  

  * Language を "Swift" に設定

  ![プロジェクト作成2](http://sunsunsoft.com/image/memo/swift_project2.png)

##scanf
<!-- scanf:: -->
コンソールからユーザーの入力を受け付ける。C言語のscanfのようなもの
~~~swift
// コンソールからユーザーの入力を受け付ける関数
func input() -> String {
    var keyboard = NSFileHandle.fileHandleWithStandardInput()
    var inputData = keyboard.availableData
    var strData = NSString(data: inputData, encoding: NSUTF8StringEncoding)!
    
    return strData.stringByTrimmingCharactersInSet(NSCharacterSet.newlineCharacterSet())
}

let command = input()
switch command {
  case "hoge"
    // ユーザーから"hoge"と入力された時の処理
    break
}

~~~

#列挙方 enum
<!-- enum:: -->
C言語のenumのようなもの。swiftのenumは高機能。  
swiftのenumの特徴
  

##基本
~~~swift
enum Signal {
    case Blue       // 青
    case Yellow     // 黄
    case Red        // 赤
}
    
// １行に書く場合
enum Signal {
    case Blue, Yellow, Red
}
~~~

## 型を指定
Swiftのenumは型を指定できる。他の言語のenumは整数値しかもてないが
~~~swift
  // 整数
  enum Signal : Int {
      case Blue       // 青
      case Yellow     // 黄
      case Red        // 赤
  }

  // 文字列を返す
  enum SignalStr : String {
      case Blue = "青"
      case Yellow = "黄"
      case Red = "赤"
  }
~~~

##メソッドを定義
enum内に関数を定義して機能を持たせることができる

~~~swift
  enum Signal {
      case Blue
      case Yellow
      case Red
      func meaning() -> String {
          switch(self) {
          case .Blue:
              return "進め"
          case .Yellow:
              return "注意"
          case .Red:
              return "止れ"
          }
      }
  }

  // enum型の変数に.をつけて呼び出す
  let s = SignalF.Yellow
  print("meaning:" + s.meaning())
~~~

##参照
~~~swift
  // enum型.値
  let signal1 = Signal.Blue
  let signal2 = Signal.Red

  // 関数の呼び出し
  let s = Signal.Yello
  print(s.meaning)

  // 関連値
  // あまり使わなさそうなのでスキップ

  // switch文
  let s = SignalF.Yellow
  switch s {
  case .Blue:
    print("青")
  case .Yellow:
    print("黄")
  case .Red:
    print("赤")
  }
~~~

#構造体 struct
<!-- struct:: -->
主にデータを保持するために特化したクラス。
機能的には継承、デストラクタが使用できないクラス。
イニシャライザはinitをオーバーライドしない場合は全パラメータを引数で受け取るイニシャライザを自動で生成してくれる。

~~~swift
  // 宣言と参照
  struct Author {
    var name: String = "不明"
    var birthday: NSDate?

    func description() {
      print("name:\(name) birthday:\(birth)")
    }

    // 構造体の値を書き換えるメソッドは先頭にmutatingをつける必要がある
    mutating func flipY() {
      x = -x
    }

  }
  let author = Author()
  let name = author.name
  let birthday : NSDate = author.birthday!
~~~

#プロトコル protocol
<!-- protocol -->
Objective-Cのプロトコルとだいたい同じでメソッドと算出型プロパティのみ定義可能、プロパティは持てない
プロトコルの特徴

  * プロトコルの宣言ではインターフェースの定義のみ。実装は書かない
  * メンバ変数(プロパティ)は持てない
  * １つのクラスに複数のプロトコルを適用することができる

##プロトコルの宣言
~~~swift
protocol Protocol1 {
  // メソッド
  func hoge()
  func hoge2() -> String

  // 算出型プロパティ get/set を定義すると設定、参照の属性を指定できる
  var str1 : String {get}   // 参照のみ
  var str2 : String {get/set}   // 参照/設定
}
~~~

##プロトコルの実装
プロトコルを実装するには実装先のクラス宣言時に : の右側にプロトコル名を追加する
~~~swift
// class クラス名 : プロトコル名
// class クラス名 : 親クラス名, プロトコル名1, プロトコル名2...

class Enemy : Protocol1 {
  var _str1 : String
  var _str2 : String

  // 実装 (プロトコルのメソッドを全部実装する)
  func hoge() { print("hoge") }
  func hoge2() -> String {
    return "hogehoge"
  }
  var str1 : String {
    return self._str1
  }
  var str2 : String {
    get {
      return self._str1
    }
    set(str) {
      self._str2 = str
    }
  }
}
~~~

##subscript 配列にアクセスするための定義
<!-- subscript:: -->
クラスのオブジェクトに対して [] アクセスされた時の処理を定義できる。見た目が配列っぽいけど機能は異なるため注意。
擬似的に要素番号が連続していない配列などを作る事ができる。

~~~swift
class TestSubscript
{
    var names : [String]
    init() {
        self.names = []
    }
    
    subscript(index: Int) -> String {
        // hensuu[index] で参照された時の処理 String を返す
        get {
            if index < 0 || index >= names.count {
                return ""
            }
            return names[index]
        }
        // hensuu[index] = name で代入された時の処理
        set(name) {
            if index < 0 || index > names.count {
                return
            }
            names.insert(name, atIndex: index)
        }
    }
}

// 使用
let test1 = TestSubscript()
test1[0] = "111"
test1[0] = "222"
print(test1[0])  // 222
print(test1[1])  // 111
~~~

#標準のクラスを拡張する
swiftでは既存のクラス（標準クラスを含む）

#ジェネリクス
C++のテンプレートのようなもの
型が違うだけで処理は同じような場合に、ジェネリックでどの型にも対応する処理を作成できる

~~~swift
  func bigger<T:Comparable>(val1: T, val2: T) -> T {
    return val1 > val2 ? val1 : val2
  }

  // Intの変数
  var i1 = 10
  var i2 = 20
  print(bigger(i1, val2:i2))
  
  // Floatの変数
  var f1 : Float = 10.0
  var f2 : Float = 20.0
  print(bigger(f1,val2:f2))
  
  var d1 : Double = 10.0
  var d2 : Double = 20.0
  print(bigger(d1, val2:d2))
  
  var s1 : String = "Mickey"
  var s2 : String = "Miffy"
  print(bigger(s1,val2:s2))
~~~

# シングルトン singleton
<!-- singleton:: -->
遅延初期化＆スレッドセーフ対応版
Swiftはstatic変数が使用できないので、
~~~swift
class Singleton {
    class var sharedInstance : Singleton {
        struct Static {
            static let instance : Singleton = Singleton()
        }
        return Static.instance
    }
}

var single : Singleton = Singleton.sharedInstance;
~~~

#演算子のオーバーロード overload
<!-- operator:: overload:: -->

  * IntやFloat等のプリミティブ型以外の型（構造体）を演算子を使用して計算することが出来る。
  * クラス同士を演算子(+,-,*,/等)で処理できる

~~~swift
  // 演算する構造体
  struct Vector3D {
    var x : Float = 0, y : Float = 0, z : Float = 0
    var description : String {
        return "x:\(x) y:\(y) z:\(z)"
    }
  }

  // 二項演算子   A + B
  func + (left: Vector3D, right: Vector3D) -> Vector3D {
      return Vector3D(x:left.x + right.x,
          y:left.y + right.y,
          z:left.z + right.z)
  }
  //単項演算子   -B
  prefix func - (vector : Vector3D) -> Vector3D{
      return Vector3D(x:-vector.x, y:-vector.y, z:-vector.z)
  }

  //複合代入演算子 A += B
  func += (inout left : Vector3D, right : Vector3D){
      left = left + right
  }

  // その他、減算'-' やかけ算'*' を行う事もできる
~~~

#タイプエイリアス typealias
<!-- typealias -->
タイプエイリアスとは、型の別名定義のことです。
C言語のtypedefのように、既存の型に別名をつけて使用することができます。

~~~swift
  typealias YorN = Bool
  typealias FuncType = (Int, Int)->Int

  typealias Money = Int
  let m: Money = 100
  print(m)  // 100

  typealias BodyStatus = (String, Double)
  var height:BodyStatus?
  var weight:BodyStatus?

  height = ("身長", 170.0)
  weight = ("体重", 63.5)
~~~

#ネストした型
Swiftではクラスや構造体、列挙型の定義の中に、さらにクラスや構造体、列挙型を定義することができます。型をネストして定義

~~~swift
  class CNest1 {
      class CNest2 {
          class CNest3 {
              func test1() {
                  print("CNest3:test1")
              }
          }
          var nest3 : CNest3 = CNest3()
          func test1() {
              nest3.test1()
              print("CNest2:test1")
          }
      }
      var nest2 : CNest2 = CNest2()
      func test1() {
          nest2.test1()
          print("CNest1:test1")
      }
  }

  var nest1 : CNest1 = CNest1()
//    var nest2 : CNest2 = CNest2()     これはできない。CNest2が使用できるのはCNest1のスコープ内だけ
~~~

#テクニック
<!-- technique:: -->
いろいろなテクニックや知識

##swiftでプリプロセッサを使用する
  `#if`系のプリプロセッサが使用出来る。`#define`系は使用できない。  
`Build Settings - [Other Swift Flags]` に以下のパラメータを追加する。

    -D DEBUG

.swift の処理
~~~swift
#if DEBUG
  // OK
#endif

#if true
  // これもOK
#endif
~~~  


#Objective-Cの移植
  AppDelegateのオブジェクトを取得
  let app = UIApplication.sharedApplication().delegate as! AppDelegate;

  //新しい
~~~swift
//[[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
window = UIWindow(frame:UIScreen.mainScreen().bounds);

//self.viewController1 = [[UNViewController1 alloc]initWithNibName:@"UNViewController1" bundle: nil];
viewController1 = ViewController(nibName: "ViewController", bundle: nil);

//self.window.rootViewController = self.viewController1;
window!.rootViewController = viewController1;

//[self.window makeKeyAndVisible];
window?.makeKeyAndVisible();
~~~
