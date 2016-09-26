#エクステンション extension
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

```swift
extension 拡張元のクラス名 {
    // 新たなメソッドの追加やプロトコルへの適応
}
```

エクステンションにより新たなプロトコルへ適応させる場合は、次の様に記述します。

```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // プロトコルへ適応するためのメソッド
}
```

####拡張例

```swift
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
```