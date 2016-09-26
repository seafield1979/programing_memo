##演算子のオーバーロード overload

  * IntやFloat等のプリミティブ型以外の型（構造体）を演算子を使用して計算することが出来る。
  * クラス同士を演算子(+,-,*,/等)で処理できる

```swift
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
```

