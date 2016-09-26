# シングルトン singleton

遅延初期化＆スレッドセーフ対応版。アプリ内で１つしかインスタンスが作成されないようなクラスはシングルトンとして実装する。

**シングルトンの特徴**

* アプリ内でインスタンスが１つしか作られない
* マルチスレッド環境でも同様
* シングルトンオブジェクトを返すメソッド`sharedInstance`で生成する

```swift
class Singleton {
    class var sharedInstance : Singleton {
        struct Static {
            static let instance : Singleton = Singleton()
        }
        return Static.instance
    }
    
    // メンバ変数
    var message : NSString?
    
    // イニシャライザ
    private override init() {
        message = "Hello World"
    }
}

var single : Singleton = Singleton.sharedInstance;
```