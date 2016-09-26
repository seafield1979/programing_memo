#swiftでプリプロセッサを使用する
`#if`系のプリプロセッサが使用出来る。`#define`系は使用できない。  
`Build Settings - [Other Swift Flags]` に以下のパラメータを追加する。

    -D DEBUG

.swift の処理

```swift
#if DEBUG
  // OK
#endif

#if true
  // これもOK
#endif
```

