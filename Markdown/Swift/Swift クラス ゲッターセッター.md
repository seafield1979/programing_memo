#ゲッターセッター  getter/setter

クラスの外部からプロパティを参照、代入された時に呼ばれるメソッドをゲッター、セッターとして定義することができる。  
**ゲッター** 参照時に呼ばれるメソッド  
**セッター** 代入時に呼ばれるメソッド  
Objective-Cと異なり、メンバ変数と直接関連はないので自由に命名できる

```swift
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
```

  ※ _hogeとhogeは直接的な関連はないので全く異なる名前でもOK。

   ゲッターだけの場合はスコープを省略できる

```swift
  var age : Int {
    return 100
  }
```

##値変更の監視
値が変更される前と後で指定のメソッドが呼ばれる。ただし、イニシャライザ内で変更される場合は呼ばれない。
willSet: 値設定前に呼ばれる
didSet: 値設定後に呼ばれる

```swift
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
```

##ゲッターセッターと値の監視を共存

```swift
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
```