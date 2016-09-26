#console

Swiftのコンソールプログラムの作り方。
引数の受け取りとユーザーの入力を受け付けて何かしら処理を行う方法を説明する。

##swiftのコンソールプログラム作成
  * メニューの [File] - [New] - [Project]
    もしくは Cmd + Shift + N  
  * 左メニューの [OS X] - Application を選択  
  * 右リストの Command Line Tool を選択  
  * 右下の [Next] ボタンを押す  

  ![プロジェクト作成1](http://sunsunsoft.com/image/memo/swift_project.png)  

  * Language を "Swift" に設定

  ![プロジェクト作成2](http://sunsunsoft.com/image/memo/swift_project2.png)

##ユーザーの入力を受け取る  scanfっぽいやつ

```swift
/*
 * コンソールでユーザーの入力を取得する
 *
 *  hoge.1 みたいに [コマンド名].[モード番号] で返す
 */
func getInput() -> (name:String, mode:Int) {
    print("\nplease input test name! ")

    let keyboard = FileHandle.standardInput
    let inputData = keyboard.availableData
    let strData = NSString(data: inputData, encoding: 4)!  // 4->NSUTF8StringEncoding
    let command = strData.trimmingCharacters(in: CharacterSet.newlines)
    
    let splited = command.components(separatedBy: ".")
    if splited.count >= 2 {
        if let testNo = Int(splited[1]) {
            return (splited[0], testNo)
        }
    }
    
    return (command, 1)
}

let command = getInput()
command.name   // String コマンド名が文字列で入る
command.mode   // Int モードが整数値で入っている
```

