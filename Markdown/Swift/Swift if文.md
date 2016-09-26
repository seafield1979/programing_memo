#if文 if
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

```swift
if a = b {
}
```

はエラー`( type '()' does not conform to protocol 'Boolean Type')` になる。
if文の条件式には必ずBoolean型の値を与えてやらないとダメみたい。
以下もだめ

```swift
var a:Int = 1
if a {

}
```