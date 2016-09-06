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
[UserDefaultViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/StorageTest/StorageTest/UserDefaultViewController.swift) | NSUserDefaultを使用したデータの保存、読み込み

##NSUserDefaults
ちょっとした設定やフラグ等を手軽に保存する。

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
    let ud = NSUserDefaults.standardUserDefaults()
    let str = ud.stringForKey(key)
    
// データの削除
    ud.removeObjectForKey("hoge")

// 全値の取得＆表示
    // 設定値すべてを取得　※ システムで用意された設定値も出力されます
    let dictionary : NSDictionary =  ud.dictionaryRepresentation()
    print(dictionary)

~~~

##CoreData
いわゆるデータベース。データベースにテーブルを作成してそこにデータを読み書きする。大量のユーザー情報や商品情報などを保持するのに使用。

##NSFileManager
ユーザーが作成したテキストや画像等を保存する。各アプリごとに用意されたDocuments以下にのファイルにしかアクセスできない。

[NSFileManagerでAPP保存領域のデータを操作する](http://qiita.com/nomunomu/items/69632904303cf6b762dd)  

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

~~~swift
BOOL result;
NSError *error;
// NSFileManagerの取得
NSFileManager *fm = [NSFileManager defaultManager];
 
// 新規作成
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
