#is as
##is
型チェック  
インスタンスに対してチェックを行い、True/Falseを返す  
`[インスタンス] is [チェックしたいクラス]`

```swift
let array : [Any] = ["text", 10, 20, "30"]
for obj in array {
    if obj is Int {
        print("\(obj) is Int")
    }
    else if obj is String{
        print("\(obj) is String")
    }
}
```

##as
 * インスタンスをダウンキャストする
 * 数字=>文字列のような変換をするものではない
 * 共通するベースクラスにアップキャストされたインスタンスをダウンキャストする場合に使用する
 * as?とすると、変換が失敗した場合はnilを返すようにできる
 * つけないと、失敗した場合ランタイムエラーになる

// 強制的にダウンキャストする。失敗した場合はランタイムエラー
let [ダウンキャストされたインスタンス] = [インスタンス] as [ダウンキャスト先のクラス]
// ダウンキャスを試みて、失敗した場合はnilを返す
let [ダウンキャストされたインスタンス(失敗すればnilが入る)] = [インスタンス] as? [ダウンキャスト先のクラス]

```swift
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
```