#オプショナル optional
<!-- optional -->
  [\[Swift\] Optional 型についてのまとめ Ver2](http://qiita.com/cotrpepe/items/518c4476ca957a42f5f1)

  通常の変数はnilを代入できない。nilを代入できるようにしたのがOptional型。Optional型があるとなにが便利かというと、関数呼び出し等の引数で変数を渡す時にこの変数はnilが入ってないよ、もしくは入っている可能性があるよ、というのを明示できる。これにより、無駄なnilチェック処理が省くことができる。
  
###オプショナル型の書式

```swift
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
```

###unwrapする (その1 Force Unwrap)
optional型の変数を使用する場合は hoge!  のように変数名の後ろに!をつけてアンラップし使用できるようにする。

```swift
  var a: String? = "hoge"
  print(a!)
```

###unwrapする（その２ Optional Chaining） 
!でアンラップすると変数がnilのときに実行時エラーが発生する。  
?でアンラップすると変数がnilのときにエラーが発生せずにnilが返る  
  →アンラップしたときにnilが受け取れない可能性がある場所で使用するとエラーになる。  

```swift
  var hoge : String? = nil
  print(hoge?)     // これはエラー。printの引数はnilを受け取れない
  var hoge2 : String? = hoge?.uppercaseStr  // これはOK
```

###unwrapする (その３)
?? でアンラップするとnilのときに返す値を別途設定できる

```swift
  var str : String?
  print(str ?? "nilですね")    // strがnilのときに"nilですね"が表示される
```

###nil判定 if let 
オプショナル型の値がnilかどうかで処理を分岐したい場合の書式。単純にif文でnilチェックをする事もできるが、Swiftでは if文の中で letに代入した結果の判定でnilチェックする方法が用意されている。

```swift
  var hoge : Hoge? = nil
  if hoge != nil {}            // そのまま
  
  if let hogehoge = hoge {    // hogeがnil以外なら真
    print(hogehoge)     // 真のブロックで hogehoge が使用できる
  } else {
    print("hogeはnilだ");
  }
```

