<!-- Stylesheet -->
<link href="http://kevinburke.bitbucket.org/markdowncss/markdown.css" rel="stylesheet"></link>
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">SwiftでiOSアプリ作成</span>

<!-- もくじ -->
[TOC]


#ストレージ
アプリを終了してもデータを保持できる仕組み

##GitHubサンプルプログラム

|ファイル名|説明|
|---|---|
[UserDefaultViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/StorageTest/StorageTest/UserDefaultViewController.swift) | NSUserDefault
[FileManagerViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/StorageTest/StorageTest/FileManagerViewController.swift) | FileManagerサンプル
[MemoStoreViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/StorageTest/StorageTest/MemoStoreViewController.swift)  | CoreDataサンプル



##NSUserDefaults

ちょっとした設定やフラグ等を手軽に保存する。

###メソッドとか
NSUserDefaults.standardUserDefaults() で取得したオブジェクトを使用して各種メソッドを呼び出す
~~~swift
let ud = NSUserDefaults.standardUserDefaults()
ud.setObject("babababa", forKey: "key123")
~~~

**設定系**

|メソッド|説明|
|---|---|
setInteger(value : Int, forKey: String) | int型のデータを保存する
setFloat(value : Float, forKey: String) | float型のデータを保存する
setDouble(value : Double, forKey: String) | float型のデータを保存する
setBool(value : Bool, forKey: String) | float型のデータを保存する
setObject(value : AnyObject?, forKey: String) | String, Number, Array, Dictionary 等のデータを保存する
synchronize | 保存する情報を即時に反映させる

**取得系**

|メソッド|説明|
|---|---|
integerForKey(defaultName: String) -> Int | int型としてデータを読み込む
floatForKey(defaultName: String) -> Float | float型としてデータを読み込む
doubleForKey(defaultName: String) -> Double | double型としてデータを読み込む
boolForKey(defaultName: String) -> Bool | BOOL型としてデータを読み込む
stringForKey(defaultName: String) -> String? | String型としてデータを読み込む
arrayForKey(defaultName: String) -> [AnyObject]? | Array型としてデータを読み込む
dataForKey(defaultName: String) -> NSData? | NSData方を取得

**その他**

|メソッド|説明|
|---|---|
removeObjectForKey(defaultName: String) | データを削除する
dictionaryRepresentation() -> [String : AnyObject] | 全データを取得する

###サンプル
~~~swift
// NSUserDefaultオブジェクト取得
    let ud = NSUserDefaults.standardUserDefaults()

// 設定 key = value
    let key = "hoge"
    let value = "12345"
    ud.setObject(value, forKey: key)
    // すぐにシステムに反映(ストレージに保存)
    ud.synchronize()

// 取得(文字列)
    let key = "hoge"
    let str = ud.stringForKey(key)
    
// データの削除
    ud.removeObjectForKey("hoge")

// 全値の取得＆表示
    // 設定値すべてを取得　※ システムで用意された設定値も出力される
    let dictionary : NSDictionary =  ud.dictionaryRepresentation()
    print(dictionary)

    for obj in dictionary {
        let value = obj.value
        //print(obj.key.dynamicType)
        
        if value is NSString {
            print(obj.key.description + "(NSString)")
            print("    " + value.description)
        }
        else if value is NSArray {
            print(obj.key.description + "(NSArray)")
            let objv  = obj.value as? NSArray
            print("{")
            for obj2 in objv! {
                print("    " + obj2.description)
            }
            print("}")
        }
        else if value is Int {
            print(obj.key.description + "(Int)")
            print("    " + value.description)
        }
        else {
            print(obj.key)
            print(obj.value.dynamicType)
        }
    }
~~~

##CoreData
いわゆるデータベース。データベースにテーブルを作成してそこにデータを読み書きする。大量のユーザー情報や商品情報などを保持するのに使用。

[SwiftでCoreDataを使ってみる（Stepごとに解説）](http://qiita.com/mochizukikotaro/items/0355131c0df268454673)

[iOSアプリでデータベース（CoreData）を使う時に必要な知識をまとめてみた - Swift編 -](http://qiita.com/merrill/items/2a3588aa98cc64b5dea0)

※注意点
一旦CoreDataを使うプログラムを実行した後にエンテティに変更を加える(カラム追加、削除等)と、次回実行時に  
NSEntityDescription.insertNewObjectForEntityForName  
を実行した時にアプリが落ちる。対処方法としては、エンティティに変更を加えたらアプリを一旦削除してから再実行する。すでにリリース済み等でアプリを削除できない場合はXCodeからCoreDataのバージョン移行を行う

###プロジェクト準備

###検索の条件指定
`let predicate = NSPredicate(format: "name = %@", nameTextField.text!)`
のように検索条件を指定する際にオペレータ(上記では=の部分)に以下のものが指定できる。

|オペレータ|説明|
|---|---|
= | 文字列完全一致
beginswith | 先頭部分一致
endswith | 末尾部分一致
like | 正規表現
contains | 文字列を含む

~~~swift
// 例
// LIKE 
let predicate = NSPredicate(format: "name LIKE[c] %@", _name)
// CONTAINS
let predicate = NSPredicate(format: "name contains[c] %@", _name)
// BEGINSWITH
let predicate = NSPredicate(format: "name beginswith[c] %@", _name)
// ENDSWITH
let predicate = NSPredicate(format: "name endswith[c] %@", _name)
~~~

##NSFileManager
ユーザーが作成したテキストや画像等を保存する。各アプリごとに用意されたDocuments以下にのファイルにしかアクセスできない。

↓わかりやすい説明、最初にここを読んでおくとよい  
<a href="http://qiita.com/nomunomu/items/69632904303cf6b762dd" target="_blank">NSFileManagerでAPP保存領域のデータを操作する</a>

~~~
ディレクトリ構成
MyApp.app/
　┠ Document/ 
　│ 　┗ Inbox/
　┠ Library/
　│ 　┠ Caches/
　│ 　┗ Preferenses/
　┗ tmp/
~~~

###サンプル
~~~swift

// ファイル書き込み
let file_name = "hoge.txt"
let text = "hogehoge"

// Documentsフォルダパスを取得
if let dir : NSString = NSSearchPathForDirectoriesInDomains( NSSearchPathDirectory.DocumentDirectory, NSSearchPathDomainMask.AllDomainsMask, true ).first
{
    print(dir)
    let path_file_name = dir.stringByAppendingPathComponent( file_name )
    
    do {
        // ファイルに書き込む
        try text.writeToFile( path_file_name, atomically: false, encoding: NSUTF8StringEncoding )
        
    } catch {
        //エラー処理
    }
}


//ファイル読み込み
let text = try! NSString( contentsOfFile: filePath, encoding: NSUTF8Str]ingEncoding )


// ファイル削除
try! NSFileManager.defaultManager().removeItemAtPath("\(path)/\(fileName)")


// フォルダ作成
if let dir = NSSearchPathForDirectoriesInDomains( NSSearchPathDirectory.DocumentDirectory, NSSearchPathDomainMask.AllDomainsMask, true ).first as String?
{
    let createPath = dir + "/" + dirName    // 作成するディレクトリ名を含んだフルパス
    
    do {
        try NSFileManager.defaultManager().createDirectoryAtPath(createPath, withIntermediateDirectories: true, attributes: nil)
    } catch {
        // Faild to wite folder
    }
}
~~~

<br>
いろいろな処理のサンプル
~~~swift
// ファイル作成(バイナリ)
let writeData = "hogehoge".dataUsingEncoding(NSUTF8StringEncoding)
NSFileManager.defaultManager().createFileAtPath( filePath, contents: writeData, attributes: nil)

// ファイルの存在確認（ディレクトリも同じ）
let result = NSFileManager.defaultManager().fileExistsAtPath(filePath)
 
// ファイルの削除（ディレクトリも同じ）
try! NSFileManager.defaultManager().removeItemAtPath(filePath)
 
// ファイルの移動（ディレクトリも同じ）
try! NSFileManager.defaultManager().moveItemAtPath(srcPath, toPath: dstPath)

// ファイルのコピー（ディレクトリも同じ）
result = [fm copyItemAtPath: src toPath:dst error:&error];
 
// ディレクトリの作成
result = [fm createDirectoryAtPath:path withIntermediateDirectories:NO attributes:nil error:&error];
 
// ディレクトリかどうか
BOOL dir;
if ( [fm fileExistsAtPath:path isDirectory:&dir] ){
   if ( dir ){
        // path is directory.
   }
}
 
// ディレクトリ内のファイル列挙
for ( NSString *path in [fm contentsOfDirectoryAtPath:dst error:&error] ){
    // ....
}
 
// NSErrorがnilでなければエラーが発生している
NSLog(@"%@.",[err localizedDescription]);
~~~

#AlertView
<!-- alert:: actionsheet: -->
以前は
ただのポップアップ -> UIAlertView  
複数ボタンのポップアップ -> UIActionSheet  
を使っていたが、ともにiOS9で廃止されることになった。iOS8以上なら UIAlertControllerが使用できるのでこちらを使用すること。ただし、iOS7以下でも動作するアプリを開発する場合は UIAlertControllerが使えないので、上記のクラスを使用するしかない。  
  
**サンプル**  
[AlertViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/UIViewTest/UIViewTest/ViewController/AlertViewController.swift)

**OKボタンだけのポップアップ**  
UIAlertController(iOS8以降)  
UIAlertView(iOS7以前)  
![UIPickerView](http://sunsunsoft.com/image/ios/alert1.png)  

**ボタンがたくさんあるポップアップ**  
UIAlertController(iOS8以降)  
UIActionSheet(iOS7以前)  
![UIPickerView](http://sunsunsoft.com/image/ios/alert2.png)


#nib
<!-- nib:: xib:: -->

自前のnibファイルからUIView(とそのサブクラス)を生成する方法

[[Swift]xibファイルを呼び出す最も簡単な方法](http://qiita.com/iKichiemon/items/3cfa6c2bf2a0acb299a0)
  
###サンプル  
[GitHub NibText](https://github.com/seafield1979/ios_programing/tree/develop/swift/NibTest/NibTest)  
※サンプルを見て確認するのが一番早い

~~~swift
// MyView.xibからViewを生成する
let myView = UINib(nibName: "MyView", bundle: nil).instantiateWithOwner(self, options: nil)[0] as! UIView
self.view.addSubview(myView)
~~~

###メモ

  * UIViewのサブクラスのソースを作成(->CustomViewとか)
  * xibファイルを作成する(CustomView.xibとか)
  * xib編集
    - ファイルオーナーにCustomViewを設定
    - メインのView以下にUIViewを追加。ボタンやラベル等の自前のViewを追加
    - CustomView.swiftにCustomView.xibからIBOutletやIBActionを追加
    - ★autolayoutを削除する(削除しないと表示サイズがおかしくなる)

  * CustomView.swift編集
    - initの中で xib のviewを自身に追加(addSubview)する

  * ViewController.swift
    - CustomViewを生成、viewに追加する
    - CustomViewのIBOutlet、IBActionを設定

#UITabBarController

以下のサンプルを参考にする
[GitHub TabBarView](https://github.com/seafield1979/ios_programing/tree/develop/swift/ViewController/ViewController/TabBarView)  

基本は

 * UITabBarController のサブクラスを作成する(xibは作らない)
 * UITabBarControllerのviewDidLoadでTabBarに表示するViewControllerを作成し、setViewControllersで設定する
 * UITabBarControllerのTabBarを追加する(いくつか方法がある)

~~~swift
class MyTabBarController: UITabBarController {
    override func viewDidLoad() {
        super.viewDidLoad()
      
        Tab1View = Tab1ViewController()
        Tab2View = Tab2ViewController()
        Tab3View = Tab3ViewController()
        Tab4View = Tab4ViewController()
                
        // タブで表示するViewControllerを配列に格納する
        let myTabs = [Tab1View!, Tab2View!, Tab3View!, Tab4View!]
        
        // 配列をTabにセット
        self.setViewControllers(myTabs, animated: false)

        // TabBarにアイコンを設定
        Tab1View.tabBarItem = UITabBarItem(tabBarSystemItem: UITabBarSystemItem.Featured, tag: 1)
        Tab2View.tabBarItem = UITabBarItem(tabBarSystemItem: UITabBarSystemItem.Bookmarks, tag: 2)
        Tab3View.tabBarItem = UITabBarItem(tabBarSystemItem: UITabBarSystemItem.Search, tag: 3)
        Tab4View.tabBarItem = UITabBarItem(tabBarSystemItem: UITabBarSystemItem.Downloads, tag: 4)
    }
}
~~~

#CGContext
CGContextを使用すると独自の描画メソッドを使用して描画、結果をUIImageで取得することができる。

**サンプル**  
[CGContextViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/MyImageView/MyImageView/CGContextViewController.swift)

CGContextでできること

 * 矩形描画
 * 文字描画(フォント、サイズ指定可能)
 * 画像描画(拡大、縮小) ※ただし矩形の切り出しはできない
 * 円、線、曲線描画

CGContextの処理手順

 1. CGContextのサイズを決めて開く
 2. CGContextを取得<br>（必要であれば）CGContextのtransformを設定
 3. Draw／Render系メソッドを使ってCGContextに画像を描画
 4. CGContextから描画済の画像を取得
 5. CGContextを閉じる

[[Objective-C] 画像処理についてのまとめ](http://qiita.com/edo_m18/items/0cdea3f5483a2e5dc16f)

###シンプルなCGContext使用方法
**コンテキストを使用しない(図形の描画をしない)**
~~~swift
func creaeImage(image :UIImage) ->UIImage
{
    // 画像コンテキスト作成(Canvasみたいなもの)
    let imageRect = CGRectMake(0,0,image.size.width,image.size.height)
    
    UIGraphicsBeginImageContext(image.size);
    
    // コンテキストにUIImageを描画
    image.drawInRect(imageRect)

    // 出力UIImageを取得
    let newImage = UIGraphicsGetImageFromCurrentImageContext();
    
    // コンテキストを解放
    UIGraphicsEndImageContext()
    
    return newImage
}
~~~

**コンテキストを使用する(図形を描画する)**
~~~swift
func createRectImage(size : CGSize) -> UIImage
{
    // コンテキストを開く
    UIGraphicsBeginImageContextWithOptions(size, false, 0)
    // コンテキストを得る
    let context:CGContextRef = UIGraphicsGetCurrentContext()!
    
    // ■を描画 (CGContextFillRect)
    // この塗りつぶす領域の大きさを指定
    let rect:CGRect = CGRectMake(0, 0, size.width, size.height)
    //　色をRGBAで指定 (r,g,b)
    CGContextSetRGBFillColor(context,CGFloat(0) / 255,CGFloat(0) / 255,CGFloat(255) / 255, 1.0)
    // 指定された領域を塗りつぶす
    CGContextFillRect(context, rect)
    
    // コンテキストに描画済の画像を得る
    let newImage = UIGraphicsGetImageFromCurrentImageContext()
    // コンテキストを解放
    UIGraphicsEndImageContext()
    
    return newImage
}
~~~

**画像描画**

#http通信

##NSURLSession
###同期型
通信が終わるまで処理を待つタイプ。

~~~swift
func testSynchronism() {
    // create the url-request
    let urlString = testURL
    let request = NSMutableURLRequest(URL: NSURL(string: urlString)!)

    // set the method(HTTP-GET)
    request.HTTPMethod = "GET"

    // use NSURLSessionDataTask
    let task = NSURLSession.sharedSession().dataTaskWithRequest(request, completionHandler: { data, response, error in
        if (error == nil) {
            let result = NSString(data: data!, encoding: NSUTF8StringEncoding)!
            print(result)
        } else {
            print(error)
        }
    })
    task.resume()
}
 //レスポンス取得後の処理
func getHttp(res:NSURLResponse?,data:NSData?,error:NSError?)
{
    let response:NSString = NSString(data: data!, encoding: NSUTF8StringEncoding)!
    print(response)
}

~~~

###非同期型

通信が終わるまで処理を待たないタイプ。

~~~swift
class ViewController: UIViewController, NSURLSessionTaskDelegate, NSURLSessionDataDelegate {

func testAsynchronous() {
    // 送信したいURLを作成し、Requestを作成します。
    let url = NSURL(string:testURL)
    let request = NSURLRequest(URL: url!)
    
    // NSURLSessionConfigurationでhttp通信を接続
    let configuration = NSURLSessionConfiguration.defaultSessionConfiguration();
    let session = NSURLSession(configuration: configuration, delegate: self, delegateQueue: NSOperationQueue.mainQueue())
        
    let task:NSURLSessionDataTask = session.dataTaskWithRequest(request)
    task.resume()
}

/**
 * HTTPリクエストのデリゲートメソッド(データ受け取り初期処理)
 */
func URLSession(session: NSURLSession, dataTask: NSURLSessionDataTask, didReceiveResponse response: NSURLResponse, completionHandler: (NSURLSessionResponseDisposition) -> Void)
{
    let httpResponse = response as! NSHTTPURLResponse;
    
    responseData = NSMutableData()
    
    // Headerの情報の確認
    if (httpResponse.statusCode == 200) {
        
        completionHandler(NSURLSessionResponseDisposition.Allow);   // 続ける
        
    } else {
        // error.code = -999で終了メソッドが呼ばれる
        completionHandler(NSURLSessionResponseDisposition.Cancel);  // 止める
    }
}

/**
 * HTTPリクエストのデリゲートメソッド(受信の度に実行)
 */
func URLSession(session: NSURLSession, dataTask: NSURLSessionDataTask, didReceiveData data: NSData) {
    responseData!.appendData(data)
}

/**
 * HTTPリクエストのデリゲートメソッド(完了処理)
 */
func URLSession(session: NSURLSession, task: NSURLSessionTask, didCompleteWithError error: NSError?) {
    if let _error = error {
        print(_error)
    }
    else {
        let str = String(NSString(data: responseData, encoding:NSUTF8StringEncoding)!)
        textView1.text = str
        print(str)
        // ファイルに保存してみる
        saveDataToFile(responseData, fileName: "hoge.txt")
    }
}
}
~~~

#AFNetworking

###AFNetworkingのインストール
結論から言うとpodからのインストールだとAFNetworkingのソースが読み込めないため使えなかった。Githubから自前でAFNetworkingのソースを取得し、プロジェクトに組み込む。
しかも、最新のバージョン(3.x)だとWebに転がっているようなサンプルが動かないため、古いバージョン(2.x)系を使うようにする。

###リンク

|リンク|説明|
|---|---|
[SwiftとAFNetworkingの簡単サンプル](http://qiita.com/tajihiro/items/ad8606f61f3273423ea8) | GithubからダウンロードしたAFNetworkingをプロジェクトに組み込む方法。※ただし最新のAFNetworkingだとだめ。2.x系を使用すること。
[SwiftとAFNetworkingの簡単サンプル 最新情報 2015/01](http://qiita.com/tajihiro/items/294ce7440c8316962da2) | 上のリンクの方法でAFNetworkingがインストールできない場合に自前でプロジェクトに組み込む方法
[AFNetworking2.xを利用して、HTMLやテキストを取得する](http://qiita.com/hmuronaka/items/7e4f75eae1cccf1007e3) | プレーンテキストを受け取る
[sakaharaのブログ](http://sakahara.hatenablog.jp/entry/2014/06/08/211918) | AFNetworkingを使用したGET,POSTメッセージの送信

###パラメータ
送信と受信のデータをどのようなものにするかをオプションで設定できる。例えば受信データを php の echo 等のプレーンテキストで受け取る場合は  
~~~
let manager = AFHTTPRequestOperationManager()
manager.responseSerializer = AFHTTPResponseSerializer()
~~~
のように設定する。

**.requestSerializer  リクエスト(送信)オプション**

|パラメータ|説明|
|---|---|
AFHTTPRequestSerializer  | HTTPリクエストパラメータ（デフォルト）
AFJSONRequestSerializer  | JSON形式のリクエストパラメータ
AFPropertyListRequestSerializer | plist形式のリクエストパラメータ
  
**.responseSerializer  レスポンス(受信)オプション**

|パラメータ|説明|
|---|---|
AFHTTPResponseSerializer | HTTPレスポンス(プレーンテキスト)
AFJSONResponseSerializer | JSON形式のレスポンス（デフォルト）


~~~swift
let manager = AFHTTPRequestOperationManager()
manager.requestSerializer = AFHTTPRequestSerializer()  // 文字列形式で送る
manager.responseSerializer = AFJSONResponseSerializer()  // JSONオブジェクトを変換して受け取る

~~~


##サンプル
swiftのサンプルと対応するphpのサンプルもつける

###GETリクエストを送り、結果をプレーンテキストで受信
*swift*
~~~swift
// GET送信
// プレーンテキスト取得
func testAFNetworkingGET() {
    
    let manager = AFHTTPRequestOperationManager()
    manager.responseSerializer = AFHTTPResponseSerializer()
    
    // GETメッセージを作成
    let getURL = http://sunsunsoft.com/test/ios/test_get.php?param1=hoge&param2=mage"
    manager.GET(getURL, parameters: nil,
                success: {(operation, responsedObject) in
                    let str : NSString = NSString(data:responsedObject as! NSData, encoding:NSUTF8StringEncoding)!
                    print("response: \(str)")
    },
        
                failure: { (operation, error) in
                    print("error: \(error)")
    }
    )
}
~~~

*php*
~~~php
<?php
echo "GET message";

// GETメッセージを全部表示
foreach($_GET as $key=>$value) {
    echo "key=${key} value=${value}\n";
}

echo "hoge";
?>
~~~


###POSTリクエストを送り、結果をプレーンテキストで受信
**swift**
~~~swift
// POSTで送信
// プレーンテキスト受信
func testAFNetworkingPOST() {
    let manager = AFHTTPRequestOperationManager()
    manager.responseSerializer = AFHTTPResponseSerializer()
    let param:Dictionary<String, String> = ["param1" : "value1", "param2" : "value2"]
    
    manager.POST("http://sunsunsoft.com/test/ios/test_post.php", parameters: param,
                 success: {
                    (operation: AFHTTPRequestOperation!, responsedObject:AnyObject!) in
                    let str : NSString = NSString(data:responsedObject as! NSData, encoding:NSUTF8StringEncoding)!
                    print("response: \(str)")

        }, failure: {
            (operation, error) in
            print("Error: \(error)")
    })
}

~~~
**php**
~~~
<?php

echo "POST message\n";

// POSTメッセージを全部表示
foreach($_POST as $key=>$value) {
    echo "key=${key} value=${value}\n";
}

echo "hoge";

?>
~~~

###JSONをそのまま取得
**swift**
~~~swift
// JSON受信
func testAFNetworkingJSON() {
    //リクエスト
    let manager = AFHTTPRequestOperationManager()
    manager.requestSerializer = AFJSONRequestSerializer()
    
    manager.GET("http://sunsunsoft.com/test/ios/test.json", parameters: nil,
                success: {(operation: AFHTTPRequestOperation!, responsedObject: AnyObject!) in
                    print("Success!!")
                    
                    // responsedObject を表示。ただ、Dictionary型なので存在しない要素にアクセスしようとしようとすると落ちる
                    print(responsedObject)
                    print(responsedObject["a"])
                    print(responsedObject["b"])
                    // print(responsedObject[0])        // 落ちる
                    
                    // SwiftyJSONオブジェクトに変換
                    let json = JSON(responsedObject)
                    print(json)
                    print(json["a"])
                    print(json["b"])
                    print(json[0])           // 落ちない
        },
                failure: {(operation, error) in
                    print("Error!!")
        }
    )
}

~~~
**json**
~~~
{
    "a":"1",
    "b":"2",
    "c":"3"
}
~~~

###JSON形式のPOSTリクエストを送り、結果をJSON形式のテキストで受信
**swift**
~~~swift
// テキスト形式JSON受信
func testAFNetworkingJSON2() {
    //リクエスト
    let manager = AFHTTPRequestOperationManager()
    manager.responseSerializer = AFHTTPResponseSerializer()  // textを受け取る
    
    let param:Dictionary = ["param1" : "value1", "param2" : "value2",
                             "array1" : [1,2,3,4,5],
                             "dic1" : ["key1" : "123", "key2" : "hoge"]]
    
    manager.POST("http://sunsunsoft.com/test/ios/test_json.php", parameters: param,
                success: {(operation: AFHTTPRequestOperation!, responsedObject: AnyObject!) in
                    print("Success!!")
                    let str : NSString = NSString(data:responsedObject as! NSData, encoding:NSUTF8StringEncoding)!
                    print("response: \(str)")
                    
                    // print(responsedObject["param1"]);  responsedObjectはJSONオブジェクトになっていないのでエラー

                    // SwiftyJSONオブジェクトに変換
                    // phpが返すのは json_encode したJSON文字列
                    if let response = responsedObject {
                        let json = JSON(data:response as! NSData)
                        print(json["param1"])
                        print(json["param2"])
                        print(json["array1"])
                        print(json[0])           // 落ちない
                    }
        },
                failure: {(operation, error) in
                    print("Error!!")
        }
    )
}

~~~
**php**
~~~
<?php

// GETメッセージを全部表示
$json = json_encode($_POST);
print_r($json);

?>
~~~

###JSON形式のPOSTリクエストを送り、結果をJSONオブジェクトで受信
**swift**
~~~swift
// POSTしたデータをJSONオブジェクト形式で受信
func testAFNetworkingJSON3() {
    //リクエスト
    let manager = AFHTTPRequestOperationManager()
    manager.requestSerializer = AFHTTPRequestSerializer()  // 文字列形式で送る
    manager.responseSerializer = AFJSONResponseSerializer()  // JSONオブジェクトを変換して受け取る
    
    let param:Dictionary = ["param1" : "value1", "param2" : "value2",
                            "array1" : [1,2,3,4,5],
                            "dic1" : ["key1" : "123", "key2" : "hoge"]]
    
    manager.POST("http://sunsunsoft.com/test/ios/test_json2.php", parameters: param,
                 success: {(operation: AFHTTPRequestOperation!, responsedObject: AnyObject!) in
                    print("Success!!")
                    
                    print(responsedObject["param1"]);
                    
                    // SwiftyJSONオブジェクトに変換
                    // phpが返すのは json_encode したJSON文字列
                    if let response = responsedObject {
                        let json = JSON(response)
                        print(json["param1"])
                        print(json["param2"])
                        print(json["array1"])
                        print(json["dic1"])
                    }
        },
                 failure: {(operation, error) in
                    print("Error!!")
                    print(error)
        }
    )
}

~~~
**php**
~~~
<?php

// 渡ってきたPOSTデータ(json)をJSONオブジェクトに変換してから返す

// Content-Typeを「application/json」に設定します。
header("Content-Type: application/json; charset=UTF-8");

// IEがContent-Typeヘッダーを無視しないようにします(HTML以外のものをHTML扱いしてしまうことを防ぐため)
header("X-Content-Type-Options: nosniff");

// 可能な限りのエスケープを行い、JSON形式で結果を返します。
echo json_encode(
    $_POST,
    JSON_HEX_TAG | JSON_HEX_APOS | JSON_HEX_QUOT | JSON_HEX_AMP
);
?>
~~~
