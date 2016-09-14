<!-- Stylesheet -->
<link href="http://kevinburke.bitbucket.org/markdowncss/markdown.css" rel="stylesheet"></link>
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">SwiftでiOSアプリ作成</span>

<!-- もくじ -->
[TOC]

**リンク**
<!-- link:: -->
[日本語に翻訳されたiOS/watchOS/tvOSのドキュメント](https://developer.apple.com/jp/documentation/)

**Todo**
<!-- todo:: -->

* Webサーバーと通信
* カメラのテスト
* MapViewのテスト
* 高速画像描画 SpriteKit のテスト
* 実機でデバッグ(iPhone,iPad)

//* 自前の画像作成(CGContext)
//* UITabBarController
// * UIActionSheet(選択肢つきポップアップ)
// * UIAlertView(メッセージ)
//* ファイルの読み書き


#UIWindow
###Windowを取得する
~~~swift
window = UIWindow(frame:UIScreen.mainScreen().bounds)
~~~

###

#UIViewController
<!-- uiviewcontroller:: -->
###画面に表示する
windowのトップにViewControllerを表示する  

~~~swift
// UIViewControllerを生成して画面に貼り付ける(※ただしUIViewの色を変えないと見た目の変化なし)
let viewController : UIViewController()
viewController.backgroundColor = .grayColor()
window!.rootViewController = viewController

// xibファイルを元にViewControllerを生成する
// MyViewController は UIViewControllerのサブクラス
let viewController2 = MyViewController(nibName: "MyViewController", bundle: nil);
~~~

###デフォルトのviewをコードで生成する
スクリーンと同じサイズのUIViewを生成して viewに設定
これで画面サイズの異なるデバイスでも画面サイズとviewのサイズが一致する
~~~swift
class MyViewController : UIViewController {

override func loadView() {
    self.view = UIView(frame: UIScreen.mainScreen().bounds)
}

}
~~~

###xibファイルから生成
自前でレイアウトしたxibファイルからUIViewController(のサブクラス)を生成する

ViewController(nibName: "nibファイルの名前", bundle: nil)
~~~swift
// ViewController.xibから ViewController を生成する
// bundle は nilだとデフォルトのバンドルが使用される。バンドルとは画像とかxibとかのリソースが入った箱
self.viewController = ViewController(nibName: "ViewController", bundle: nil)
~~~

###UIViewControllerを切り替える


#UINavigationController
<!-- uinavigationcontroller:: -->
NavigationControllerはViewControllerの上部にバー(NavigationBar)を表示したり、複数のViewControllerをスタックしたりできる。

NavigationControllerにViewControllerを表示させたところ。NavigationBarにはタイトル、左ボタン、右ボタンを表示できる。
![UINavigationController3](http://sunsunsoft.com/image/ios/navigationcontroller3.png)

ViewControllerをスタックしたケース。前のViewControllerに戻るためのボタンがNavigationBarの左側に表示されている。  
![UINavigationController2](http://sunsunsoft.com/image/ios/navigationcontroller2.png)  


##UINavigationControllerでできること

* ナビゲーションバーにタイトル、ボタンを表示できる
* 画面をpush(前のページを保持しつつ新しいページを表示)
* 画面をpop(pushで進んだ画面を１つ前の画面に戻す)

##プロパティ、メソッド
**UINavigationController**

|メソッド・プロパティ|説明|
|---|---|
pushViewController(viewController: UIViewController, animated: Bool) | ViewControllerをPushする(新しいページ)
popViewControllerAnimated(animated: Bool) -> UIViewController? | ViewControllerをPopする(前のページ)
topViewController : UIViewController | 一番先頭のViewControllerを返す
viewControllers : [UIViewController] | 保持しているViewControllerの配列
navigationBarHidden | NavigationBarの表示状態を返す
navigationBar | NavigationBar

**UIViewController**

|メソッド・プロパティ|説明|
|---|---|
title | ナビゲーションバーに表示するタイトルを設定する
navigationController | UINavigationControllerを取得する（NavigationControllerで表示されている場合のみ有効)
navigationItem | NavigationBar項目のセット

**UINavigationBar**
ナビゲーションバー

|メソッド・プロパティ|説明|
|---|---|
tintColor | ナビゲーションバーの色を設定する
barStyle | UIBarStyle<br>.Default：灰色<br>.Black：黒色<br>.BlackTranslucent:黒透明

**UINavigationItem**
Navigationに表示する項目。タイトルや左右のボタンなどはこのクラスのサブクラス  

|メソッド・プロパティ|説明|
|---|---|
title | タイトルに表示する文字列
titleView | タイトル部分に表示するUIView
backBarButtonItem | 前のViewControllerに戻るUIBarButtonItem
leftBarButtonItem | バーの左側に表示されるボタン(backBarButtonItemが表示されている時はかぶるので使用しない)
rightBarButtonItem | バーの右側に表示されるボタン

##サンプルコード

**生成**  
UINavigationControllerを生成する場合は、そこに表示するUIViewControllerを生成する。

~~~swift
let viewController1 = ViewController(nibName: "ViewController", bundle: nil)
let navigationController = NavigationController1(rootViewController: viewController1!)
window!.rootViewController = navigationController
~~~

**その他**
~~~swift
// push
// 新しいViewControllerを生成し、それを表示する
let viewController = ViewController(nibName: "ViewController", bundle: nil)
self.navigationController?.pushViewController(viewController, animated: true)

// スタックに積まれたViewControllerのうち、指定のViewControllerの場所までpopする 
self.navigationController?.popToViewController(viewController1, animated: true)

// ルートまでpopする
self.navigationController?.popToRootViewControllerAnimated(true)

override func viewDidLoad()
{
    // タイトルの文字列を変更
    self.title = "hoge"

    // タイトルのViewを変更
    self.navigationItem.titleView = UIImageView(image: UIImage(named: "image/hoge.png"))
    
    // バーのスタイルを変更
    self.navigationController!.navigationBar.barStyle = UIBarStyle.BlackTranslucent

    // ナビゲーションを透明にする
    self.navigationController!.navigationBar.setBackgroundImage(UIImage(), forBarMetrics: .Default)
    self.navigationController!.navigationBar.shadowImage = UIImage()

    // バーの背景色(半透明)
    self.navigationController!.navigationBar.tintColor = .greenColor()
        
    // バーの背景色(半透明なし)
    self.navigationController!.navigationBar.barTintColor = .greenColor()
        
    // 戻るボタンのテキスト変更
    let backButtonItem = UIBarButtonItem(title: "< 戻る", style: .Plain, target: nil, action: nil)
    navigationItem.backBarButtonItem = backButtonItem

    // 戻るボタンの背景画像
    UIBarButtonItem.appearance().setBackButtonBackgroundImage(UIImage(named: "Back"), forState: .Normal, barMetrics: .Default)

    // バーの右にテキストボタンを追加
    let debugBarButton = UIBarButtonItem(title: "Debug", style: UIBarButtonItemStyle.Plain, target: self, action: #selector(self.debugButtonTapped(_:)))
    self.navigationItem.leftBarButtonItem = debugBarButton

    // UIImageからバーのボタンを作成、設定する
    let barButton = UIBarButtonItem(image: UIImage(named:"image/hoge2.png"), style: UIBarButtonItemStyle.Plain, target: self, action: #selector(self.buttonTapped(_:)))
    self.navigationItem.rightBarButtonItem = barButton

    // UIView(とそのサブクラス)からボタンを作成、設定する
    let imageView = UIImageView(image: UIImage(named:"image/hoge.png"))
    let barButton = UIBarButtonItem(customView: imageView)
}
~~~

#UITableViewController
<!-- uitableviewcontroller:: -->
表示データが上下に並んだリストを管理する。

##基礎
NSIndexPath  
セルのセクション番号と行番号を保持する
  * section  セクション
  * row  行

##サンプルコード
|プログラム|説明|
|-----|-----|
[SimpleTableViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/ViewController/ViewController/TableView/SimpleTableViewController.swift) | セルを表示するだけのシンプルなTableView
[SimpleTableViewController2.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/ViewController/ViewController/TableView/SimpleTableViewController2.swift) | セクションとセルを表示するシンプルなTableView
[TableViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/ViewController/ViewController/TableView/TableViewController.swift) | セクションやセルに自前のxibを表示する
[EditableTableViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/ViewController/ViewController/TableView/EditableTableViewController.swift) | セルを移動、削除できるTableView

###シンプルなUITableViewController
自前のセルを使わないシンプルなUIViewControllerのサンプル

**ファイルを追加**  
XCodeでファイルを追加  

~~~swift
SimpleTableViewController.swift

class SimpleTableViewController: UITableViewController {
    
    let cellNum = 30
    let cellHeight : CGFloat = 80.0
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // ステータスバーの上にUITableViewが表示されないように位置をずらす
        let statusBarHeight = UIApplication.sharedApplication().statusBarFrame.height
        self.tableView.contentInset.top = statusBarHeight

    }

    // MARK: - UITableViewDelegate

    // セクション数を返す
    override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 1
    }

    // 行数を返す
    override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return cellNum
    }
    
    // セルを返す
    override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell
    {
        // デフォルトで用意されたセルを返す
        let cell = UITableViewCell(style: UITableViewCellStyle.Subtitle, reuseIdentifier: "MyTestCell")
        cell.textLabel!.text = "hoge selction:\(indexPath.section) : row:\(indexPath.row)"
        cell.detailTextLabel!.text = "detail"
        
        // １つおきに色を変える
        if indexPath.row % 2 == 0 {
            cell.backgroundColor = .grayColor()
        }
        return cell

    }
    
    // MARK: - UITableViewDelegate
    
    // セルの高さを返す
    override func tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat
    {
        return cellHeight
    }
}

AppDelegate.swift

func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool
{
    window = UIWindow(frame:UIScreen.mainScreen().bounds);
    simpleTableViewController = SimpleTableViewController()
    window!.rootViewController = simpleTableViewController
}
~~~

https://github.com/seafield1979/ios_programing/blob/develop/swift/ViewController/ViewController/TableView/EditableTableViewController.swift


###自前のxibのCellを使用する

UITableViewにxibでデザインしたセルを使用する方法

UITableViewControllerでできること

 * テーブルを表示する
 * セル（テーブルに表示されるデータ）をカスタマイズできる
 * セクションのヘッダーとフッターを表示できる（セクション１、セクション１のデータ、セクション２、セクション２のデータ〜のような表示順になる）
 * テーブルに表示するセルは自前でデザインしたものを使用できる
 * テーブルの行を追加、削除できる
 * テーブルの行の並び替えができる


**ファイルを追加する**  
  [iOS] - [Cocoa Touch Class] で　Next  

  ![UITableViewCell1](http://sunsunsoft.com/image/ios/uitableviewcell_1.png)

  * Class:  [適当なCell名]
  * Subclass of : UITableViewCell
  * Also create XIB file をチェック
  * で Next、ファイル作成場所を選択してから Create
  * これで UITableViewCell のサブクラス(今回はHogeCellクラス)のswfitファイルと、xibファイルが作成される

  ![UITableViewCell1](http://sunsunsoft.com/image/ios/uitableviewcell_2.png)

**xibを編集**
    2で作成しそのままだと何も表示されないので、UILabelを配置する(->label1)。
    label1を HogeCellのクラスに連結する

  [![UITableViewCell1](http://sunsunsoft.com/image/ios/uitableviewcell_3.png)](http://sunsunsoft.com/image/ios/uitableviewcell_3.png)


**ソース編集**

~~~swift
class MyViewController: UITableViewController {

override func viewDidLoad() {
...
    // セル用のxibを登録する
    let nib = UINib(nibName: "HogeCell", bundle: nil)

    // registerNibで登録 forCellReuseIdentifierの名前はなんでもOK
    self.tableView.registerNib(nib, forCellReuseIdentifier: "cell1")
}

// UITableViewControllerのセルを返すメソッド
override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell
{
    // 自前の
    let cell = tableView.dequeueReusableCellWithIdentifier("cell1", forIndexPath: indexPath) as! MyTableViewCell
    
    // HogeCellに配置したラベル(label1)にテキストを設定する
    cell.label1.text = "hoge selction:\(indexPath.section) : row:\(indexPath.row)"
    return cell
}
~~~

###デリゲートメソッド

**UITableViewDataSource**  
テーブルの表示に必須な情報を取得するメソッド。UITableViewControllerにどのような情報をどれだけ表示したら良いかを教えてあげる。主にテーブルがロード時に呼ばれる。

|メソッド|説明|
|!--|!--|
numberOfSectionsInTableView(tableView: UITableView) -> Int | セクション数を返す（ロード時）
tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int | (指定セクションの）行数を返す
tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell | セルを返す
tableView(tableView: UITableView, titleForHeaderInSection section: Int) -> String? | セクションヘッダーのタイトルを返す
tableView(tableView: UITableView, titleForFooterInSection section: Int) -> String? | セクションフッターのタイトルを返す

**UITableViewDelegate**  
主に表示系の情報を返すメソッド

|メソッド|説明|
|!--|!--|
tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat | セルの高さを返す
tableView(tableView: UITableView, heightForHeaderInSection section: Int) -> CGFloat | セクションヘッダーの高さを返す
tableView(tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat | セクションフッターの高さを返す
tableView(tableView: UITableView, willDisplayHeaderView view: UIView, forSection section: Int) | セクションヘッダーの表示(背景色等)を設定する
tableView(tableView: UITableView, willDisplayFooterView view: UIView, forSection section: Int) | セクションフッターの表示(背景色等)を設定する


###UITableViewがステータスバーと重なる問題の対応
override func viewDidLoad() {
    // これを追加する
    self.tableView.contentInset.top = 50
}


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


#UIView(とそのSubClass)
<!-- uiview:: -->
矩形を表示するオブジェクト。とりあえず画面に何か表示したいときはこのクラスか、このクラスのサブクラスを作ってメインのViewにaddSubviewする。

##UIView

###プロパティ・メソッド
**プロパティ**

|プロパティ名|説明|
|---|---|
superview UIView? | 親View
subviews [UIView] | 子View
tag (Int) | 識別用のタグ
hidden (Bool) | trueならViewを非表示

**メソッド**

|メソッド|説明|
|---|---|
addSubview() | 子Viewを追加する
removeFromSuperview() | 親Viewから自身を削除する
drawRect(rect : CGRect) | 図形を描画する



###メインViewを画面サイズと同じにする
~~~swift
class MyViewController {
    override func loadView() {
        // スクリーンと同じサイズのUIViewを生成して viewに設定
        // これで画面サイズの異なるデバイスでも画面サイズとviewのサイズが一致する
        self.view = UIView(frame: UIScreen.mainScreen().bounds)
    }
}
~~~

###UIViewオブジェクトを生成
~~~swift
// 座標とサイズを指定してUIViewを作る
let view1 : UIView = UIView(x:0, y:0, width:100, height:100)

// そのままだと透明で見えないので色を設定
view1.backgroundColor = UIColor.redColor()

// とりあえず表示してみる(表示済みのViewにaddSubview)
window!.rootViewController!.view.addSubview(view1)
~~~

###プログラムでUIViewを追加する addSubview
<!-- addsubview:: -->
func addSubview(_ view: UIView)

`[追加先のView].addSubview([追加するView])`

~~~swift
viewController.view.addSubview(label1)
~~~

###位置・座標を変更する
~~~swift
var view : UIView?

〜〜〜

//座標を設定
view.frame = CGRectMake([x], [y], [width], [height])

//座標を移動(x+100)
view?.frame = CGRect(view?.frame!.origin.x + 100,
                        view?.frame!.origin.y,
                        view?.frame!.size.width,
                        view?.frame!.size.height)

//サイズを変更(width=100,height=200)
view?.frame = CGRect(view?.frame!.origin.x,
                        view?.frame!.origin.y,
                        100.0
                        200.0)
~~~

###表示・非表示切り替え
~~~swift
//表示


~~~

##UIScrollView
<!-- uiscrollview:: -->
スクロール可能なView。内部に表示領域より大きいView(サイズはcontentsize)を保持して、ContentOffsetで指定した位置から部分的に画面に表示する。

![UIScrollView](http://sunsunsoft.com/image/ios/scrollview.png)

~~~swift
// UIScrollViewを作成
let scrollView = UIScrollView( frame: CGRectMake( 0,0, self.view.frame.size.width, self.view.frame.size.height))

// 全体の領域(viewのサイズよりも大きくするとスクロール可能になる)
scrollView.contentSize = CGSizeMake(view.frame.size.width * CGFloat(pageMax), view.frame.size.height)

scrollView.backgroundColor = .blueColor()

// ページごとのスクロールにする
scrollView.pagingEnabled = true;

// ステータスバータップでトップにスクロールする機能をOFFにする
scrollView.scrollsToTop = false;

// delegateメソッドを使用できるようにする
scrollView.delegate = self

// 親viewに追加
self.view.addSubview(scrollView)

// UIScrollViewDelegateメソッド
// スクロール時の処理（スクロール中に毎フレーム呼ばれる）
func scrollViewDidScroll( scrollView: UIScrollView) {
    // 現在のページ数を UIPageControl に設定
    print("\(scrollView.contentOffset.x) :  \(scrollView.contentOffset.y)")
}
~~~

##UILabel
<!-- uilabel:: -->
画面にテキストを表示したい場合に使用する

###生成&表示
~~~swift
// シンプルなラベルを作成
let label1 : UILabel = UILabel(frame: CGRect(x:50, y:100, width:100, height: 50))
label1.text = "Hello World!!"
viewController.view.addSubview(label1)

// カスタマイズしたラベルを作成
let label = UILabel(frame:CGRectMake(0, 0, 100, 30))
label.text = "らべる"

// テキスト中央寄せ
label.textAlignment = NSTextAlignment.Center

// テキストの色
label.textColor = UIColor.blueColor()

// フォント
label.font = UIFont(name:"ArialHebew", size:UIFont.labelFontSize())
 
// 背景色
label.backgroundColor = UIColor.grayColor()

// ラベルの領域にテキストが収まらない場合に、フォントを最大0.5倍まで縮小する
label.adjustsFontSizeToFitWidth = true
label.minimumScaleFactor = 0.5

~~~

###フォントを変更
<!-- uifont:: -->
~~~swift
// フォントを作成
// UIFont(name:,size:) か UIFontのフォント作成関数を使用する

// ヒラギノフォント
UIFont(name:"HiraKakuProN-W3", size:UIFont.labelFontSize())

// システムの標準フォントサイズ
UIFont.systemFontOfSize(UIFont.systemFontSize())

// UIButton用のフォントサイズ
UIFont.systemFontOfSize(UIFont.buttonFontSize())

// カスタムしたフォントサイズ(20)の
UIFont.systemFontOfSize(CGFloat(20))

// Italic System Font
UIFont.italicSystemFontOfSize(UIFont.labelFontSize())

// Bold
UIFont.boldSystemFontOfSize(UIFont.labelFontSize())

~~~

##UIProgressView
<!-- uiprogressview:: -->
進捗状態を 0~100%で表示するView

~~~swift
// 生成
let progress = UIProgressView(progressViewStyle: .Default)

// 座標、サイズ設定
progress.frame = CGRectMake(pos.x, pos.y, 200, 20)

// バーの色を変更
progress.progressTintColor = UIColor.redColor()

// 進捗度(0.0~1.0)を設定
progress.progress = 0.5

// サイズを変更
progress.transform = CGAffineTransformMakeScale(1.0, 4.0)

// 進捗をアニメーションで設定
progress.setProgress( 0.7, animated: true)
~~~

##UIPageControl
<!-- uipagecontrol:: -->
現在のページを表示するView。
UIPageControl自体はシンプルで、複数の選択項目の位置の１つが選択された状態を保持するだけ。
選択項目が変更された時などに呼ばれるイベントを登録しておいてページの切り替えを実現する

~~~swift
// 生成
let pageControl = UIPageControl(frame: CGRectMake(50, 100, 200, 30))
        
// ページ数
pageControl.numberOfPages = 3

// 現在のページ
pageControl.currentPage = 0

// 現在のページを示す●の色
pageControl.currentPageIndicatorTintColor = .yellowColor()

// ページを示す●の色
pageControl.pageIndicatorTintColor = .blueColor()

// ページが変更された時のイベントを登録
pageControl.addTarget(self, action: #selector(self.pageControlChanged(_:)), forControlEvents: UIControlEvents.ValueChanged)

self.view.addSubview(pageControl)

// ページが変更された時の処理
@IBAction func pageControlChanged(sender: AnyObject) {
    if let pageControl = sender as? UIPageControl {
        print(pageControl.currentPage)
    }
}
~~~

##UIPickerView
<!-- pickerview:: -->
ドラムロール式の項目選択View。複数のドラムロールを持たせることも可能  
![UIPickerView](http://sunsunsoft.com/image/ios/pickerview.png)

deleteを使用して実現しているのでサンプルのボリュームが大きい。以下のプロジェクトを参照する。  
[github: PickerViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/UIViewTest/UIViewTest/ViewController/PickerViewController.swift)

|プロパティ|説明|
|---|---|
selectedRowInComponent(component: Int) -> Int | 指定セクションの選択番号を取得 

**テンプレート**

~~~swift
class ViewController: UIViewController, UIPickerViewDelegate, UIPickerViewDataSource {

    @IBOutlet weak var picker1: UIPickerView!
    
    private let pickerTitles: NSArray = [
        "hoge1","hoge2", "hoge3", "hoge4"
    ]
    
    override func viewDidLoad() {
        super.viewDidLoad()

        picker1.delegate = self
        picker1.dataSource = self
    }
    
    // MARK: UIPickerViewDataSource
    // pickerに表示する列数を返すデータソースメソッド.
    // (実装必須)
    func numberOfComponentsInPickerView(pickerView: UIPickerView) -> Int
    {
        return 1
    }

    //pickerに表示する行数を返すデータソースメソッド.
    // (実装必須)
    func pickerView(pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int
    {
        return pickerTitles.count
    }
    
    // MARK: UIPickerViewDelegate
    // pickerに表示する値を返すデリゲートメソッド.
    func pickerView(pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String?
    {
        return pickerTitles[row] as? String
    }
    
    // pickerが選択された際に呼ばれるデリゲートメソッド.
    func pickerView(pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int)
    {
        print("row: \(row)\n value: \(pickerTitles[row])")
    }
}
~~~

##UISegmentedControl
複数の選択項目のうち１つだけ選択できるコントロール

![UISegmentedControl](http://sunsunsoft.com/image/ios/segment.png)
~~~swift
// 生成
// 表示する項目のリストを渡して生成
let segmented = UISegmentedControl(items: ["山形","宮城","福島"])

// 座標を設定
segmented.frame = CGRectMake(0, 0, 200, 30)

// 最初に選択されている項目
segmented.selectedSegmentIndex = 0

// 色(ボタンの枠や文字)
segmented.tintColor = .redColor()

// 値が変わった時のイベントを登録
segmented.addTarget(self, action: #selector(self.segmentedValueChanged(_:)), forControlEvents: UIControlEvents.ValueChanged)

~~~

自前の画像でカスタマイズしたい場合は
[UISEGMENTEDCONTROLをオリジナルの外観にしよう](https://moto44blog.wordpress.com/2013/01/11/uisegmentedcontrol%E3%82%92%E3%82%AA%E3%83%AA%E3%82%B8%E3%83%8A%E3%83%AB%E3%81%AE%E5%A4%96%E8%A6%B3%E3%81%AB%E3%81%97%E3%82%88%E3%81%86/)

##UISlider
値をスライドして調整するコントロール。
~~~swift
// UISliderを生成
let slider = UISlider(frame:CGRectMake(0, 0, 200, 30))
// 色を設定
slider.tintColor = .redColor()

// 識別用のタグを設定
slider.tag = 2

// スライダーの最大値
slider.maximumValue = 100

// 値変更の通知タイミング(true: スライド中も通知 / false:スライド中は通知しない)
slider.continuous = false

// 値が変更された時のイベントを設定
slider.addTarget(self, action: #selector(self.sliderValueChanged(_:)), forControlEvents: .ValueChanged)

// スライド時のイベント処理
@IBAction func sliderValueChanged(sender: AnyObject) {
    if let slider = sender as? UISlider {
        print(slider.value)
    }
}
~~~

##UISwitch
<!-- uiswitch:: -->
On/Offの２つの状態をもつコントロール

~~~switch
// UISwitchを生成
let _switch = UISwitch(frame: CGRectMake(0, 0, 100, 30))
        
// Offの時の背景色
_switch.tintColor = .grayColor()

// Onの時の背景色
_switch.onTintColor = .blueColor()

// ●の色
_switch.thumbTintColor = .yellowColor()

// 初期状態(OFF)
_switch.on = false

 // 値が変更された時のイベント追加
_switch.addTarget(self, action: #selector(self.switchValueChanged(_:)), forControlEvents: .ValueChanged)
~~~

##UIStepper
<!-- uistepper:: -->
値の増減を管理するコントロール。最小値、最大値、一回の増減幅を設定できる。

~~~swift
// UIStepper生成
let stepper = UIStepper(frame: CGRectMake(0, 0, 100, 30))
// 最小値
stepper.minimumValue = 0.0
// 最大値
stepper.maximumValue = 100.0
// 増減幅
stepper.stepValue = 1.0
// 現在の値
stepper.value = 0.0
// 色
stepper.tintColor = .redColor()
// 値変更時のイベント登録
stepper.addTarget(self, action: #selector(self.stepperValueChanged(_:)), forControlEvents: .ValueChanged)
~~~ 

##UITextField
１行テキスト入力欄のコントロール。タップするとキーボードが出現して、それを使ってテキストを入力できる。
~~~swift
// 生成
let textField = UITextField(frame: CGRectMake(0, 0, 200, 50))
// テキスト
textField.text = ""

// プレースホルダー（文字が入力されていない時に表示されるテキスト）
textField.placeholder = "入力するなり"

// キーボードのタイプ
/*
  .keyboardTypeプロパティに UIKeyboardType の値を設定する
  .Default：デフォルト
  .ASCIICapable：英字
  .NumbersAndPunctuation：数字・記号
  .URL：URL用
  .EmailAddress：Email用
  .NumberPad：テンキー
  .PhonePad：電話番号用
*/
textField.keyboardType = .Default

// キーボードのReturnキーの表示
/* .returnKeyTypeプロパティに UIReturnKeyType の値を設定する
 .Default：デフォルト(「return」)
 .Go：「Go」
 .Join：「Join」
 .Next：「Next」
 .Route：「Route」
 .Search：「Search」
 .Send：「Send」
 .Done：「Done」
 .EmergencyCall：「EmergencyCall」
*/
textField.returnKeyType = .Done

// フォントを設定
textField.font = UIFont(name:"HiraKakuProN-W3", size:UIFont.labelFontSize())

// テキストの色
textField.textColor = .blackColor()

// 枠のスタイル　(None, Line, Bezel, RoundedRect)
textField.borderStyle = .RoundedRect

// テキストの寄せる方向 (Left, Right, Center)
textField.textAlignment = .Left

// クリアボタン (Never, Always, WhileEditing, UnlessEditing)
textField.clearButtonMode = .UnlessEditing

// デリゲートを指定（通常ViewControllerで処理するのでself)
textField.delegate = self


// 以下UITextFieldDelegateメソッド
// textField   テキストフィールドを編集する直前に呼び出される
func textFieldShouldBeginEditing(textField : UITextField) -> Bool
{return true}

// テキストフィールドの編集が終了する直前に呼び出される
// ret:
func textFieldShouldEndEditing(textField : UITextField) -> Bool
{return true}

// テキストフィールドを編集する直後に呼び出される
func textFieldDidBeginEditing(textField: UITextField){}
{
// textField    テキストフィールドの編集が終了する直後に呼び出される
func textFieldDidEndEditing(textField: UITextField){}

// textField    Returnボタンがタップされた時に呼ばれる
func textFieldShouldReturn(textField: UITextField) -> Bool
{
    // キーボードを閉じる
    textField.resignFirstResponder()
    return true
}

// textField    クリアボタンがタップされた時に呼ばれる
// 自動でクリアしたい場合はtrueを返す（ただし、自動クリア時はキーボードが立ち上がる）
func textFieldShouldClear(textField: UITextField) -> Bool
{
    // 自前でテキストをクリアする
    textField.text = ""
    return false
}

~~~


##UIWebView
<!-- uiwebview -->
Webページを表示するコントロール。
 
###httpのページを読み込む方法
iOS9からセキュリティの関係でデフォルトの設定ではhttpのページが読めなくなった(httpsはOK)
httpを読み込むにはプロジェクトのInfo.plistに以下の設定を追加してやる

//  ATS(App Transport Security Settings)を無効
![ボタン](http://sunsunsoft.com/image/ios/info_ats.png)
~~~xlm
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
~~~

###サンプル
~~~swift
// 生成
let webView = UIWebView()

// サイズを設定
webView.frame = CGRectMake(0,20,view.frame.size.width, view.frame.size.height - 100)

// デリゲートを設定
webView.delegate = self

// ページ読み込み(任天堂のホームページ)
// 適当なWebページを表示してみる
let requestURL = NSURL(string: "http://www.nintendo.co.jp")
let req = NSURLRequest(URL: requestURL!)
webView.loadRequest(req)


// デリゲートメソッド(UIWebViewDelegate)

// ページの読み込みリクエストを受け取った時の処理
// trueを返すとリクエストのページを読み込む
func webView(webView: UIWebView, shouldStartLoadWithRequest request: NSURLRequest, navigationType: UIWebViewNavigationType) -> Bool
{
    return true
}

// ページ読み込み完了時の処理
func webViewDidFinishLoad(webView: UIWebView) {
}
~~~

### UIWebViewプロパティ
|プロパティ|説明|
|!--|!--|
delegate | デリゲートを設定する
scalesPageToFit (boolean) | ピンチイン／アウトの可否を設定する
loading (boolean) |ページ読み込み中かどうか
canGoBack (boolean) | 前のページに戻れるかどうか
canGoForward (boolean)| 次のページに進めるかどうか

### UIWebViewメソッド
loadRequest(NSURLRequest) | 指定したページを読み込む
goBack() | 前のページに戻る
goForward() | 次のページに進む
reload() | リロードする
stopLoading() | ページの読込を中止する

##UIButton
<!-- uibutton:: -->

###リンク
ハイライト時のBG色を設定する方法。直接色を指定できないのでUIColorからUIImageを作成して設定する。  
[UIButtonハイライト時の色を指定する](http://qiita.com/akatsuki174/items/c0b8b5126b6c12f62001)

###ボタン生成のサンプル
~~~swift
func createSimpleButton() {
    // ボタン生成。デフォルトの色はテキストの色が白で背景が透明なので、背景が白の場合はテキストの色を変更すること
    let button = UIButton(frame: CGRectMake(pos.x, pos.y, 100, 30))
    button.setTitle(title, forState: UIControlState.Normal)
    button.setTitleColor( UIColor.grayColor(), forState: UIControlState.Highlighted)

    // 押された時の処理
    button.addTarget(self, action: #selector(self.buttonTapped(_:)), forControlEvents: .TouchUpInside)

    // addSubview
    view1!.addSubview(button)
}
~~~

いろいろカスタマイズしたボタン  
![ボタン](http://sunsunsoft.com/image/ios/button1.png)
~~~swift
func createButton() {
    let button = UIButton()
    //表示されるテキスト
    button.setTitle("Tap Me!", forState: .Normal)

    //テキストの色
    button.setTitleColor(UIColor.blueColor(), forState: .Normal)

    //タップした状態のテキスト
    button.setTitle("Tapped!", forState: .Highlighted)

    //タップした状態の色
    button.setTitleColor(UIColor.redColor(), forState: .Highlighted)

    //サイズ
    button.frame = CGRectMake(0, 0, 300, 50)

    //タグ番号
    button.tag = 1

    //配置場所
    button.layer.position = CGPoint(x: self.view.frame.width/2, y:100)

    //背景色
    button.backgroundColor = UIColor(red: 0.7, green: 0.2, blue: 0.2, alpha: 0.2)

    //角丸
    button.layer.cornerRadius = 10

    //ボーダー幅
    button.layer.borderWidth = 1

    //ボタンをタップした時に実行するメソッドを指定
    button.addTarget(self, action: "tapped:", forControlEvents:.TouchUpInside)

    //viewにボタンを追加する
    self.view.addSubview(button)
}
~~~

###ボタンのテキストを変更する
`button3.setTitle("ほげ", forState:UIControlState.Normal)`
forStateに設定できる状態

  |値|状態|
  |!--|!--|
  UIControlState.Normal | 通常状態(有効状態で、ボタンが押されていない時)
  UIControlState.Highlighted | ハイライト(ボタンを押した時)の状態
  UIControlState.Disabled | 無効状態 .enabled == false
  UIControlState.Selected | 選択状態 .selected == true

![ButtonState](http://i.stack.imgur.com/DDpb2.png)


##UIImageView
<!-- uiimageview:: -->

画像(UIImage)から生成
~~~swift
// プロジェクトに追加された画像 "hoge.png" から生成する
let imageView = UIImageView( image: UIImage(named: "hoge.png") )
imageView.frame.origin = CGPointMake(100.0, 200.0)
~~~

自前でUIImageViewを生成
~~~swift
// 自前のUIImageを作成して、それを元にUIImageViewを作成する
func createImageView2(pos : CGPoint, size : CGSize) -> UIImageView
{
    var imgView:UIImageView! = nil
    var img:UIImage! = nil
    
    // ここでRGBAの値を指定(今回は適当に青色にしています)
    let r:Int = 0, g:Int = 0, b:Int = 255
    let alphaValue:CGFloat = 1.0
    
    // UIImageViewを準備(iPadの横向きにフルで取ったとした場合．ご自身の要件に合わせて下さい．)
    imgView = UIImageView(frame:CGRectMake(pos.x, pos.y, size.width, size.height))
    
    // UIImageを自前で準備
    UIGraphicsBeginImageContextWithOptions(imgView.frame.size, false, 0)
    // context生成
    let contextImg:CGContextRef = UIGraphicsGetCurrentContext()!
    // この塗りつぶす領域の大きさを指定
    let rect:CGRect = CGRectMake(0, 0, size.width, size.height)
    //　色をRGBAで指定
    CGContextSetRGBFillColor(contextImg,CGFloat(r) / 255,CGFloat(g) / 255,CGFloat(b) / 255, alphaValue)
    // 指定された領域を塗りつぶす
    CGContextFillRect(contextImg, rect)
    //　現在のcontextの情報取得
    img = UIGraphicsGetImageFromCurrentImageContext()
    //　contextを終了
    UIGraphicsEndImageContext()
    
    // imgをUIImageViewで表示
    imgView.image = img
    imgView.image?.drawInRect(CGRectMake(0, 0, imgView.frame.size.width, imgView.frame.size.height))
    
    return imgView
}
~~~


###ステータスバーを非表示にする
UIViewControllerクラスに以下のメソッドを追加
~~~swift
override func prefersStatusBarHidden() -> Bool {
    return true
}
~~~


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

#ジェスチャー gesture
###ジェスチャーを追加する方法
ソースコードだけで view1:UIView をタップした時の処理を追加。

~~~swift
class GestureViewController: UIViewController {
    
    var gestureView : UIView?
    
    func tapGesture(gestureRecognizer: UITapGestureRecognizer){
        // タップviewの色を変える (Red <=> Blue)
        if(self.gestureView!.backgroundColor  == .redColor()) {
            self.gestureView!.backgroundColor = .blueColor()
        }
        else {
            self.gestureView!.backgroundColor = .redColor()
        }

    }
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Viewを追加
        self.gestureView = UIView(frame: CGRect(x:0, y:0, width:100, height:100))
        gestureView!.backgroundColor = UIColor.redColor()
        self.view.addSubview(gestureView!)
        
        // Viewにタッチイベントを追加
        self.gestureView!.userInteractionEnabled = true
        
        //ジェスチャーを作成
        let aSelector = #selector(self.tapGesture(_:))
        let recognizer : UITapGestureRecognizer = UITapGestureRecognizer(target: self, action:aSelector);
        
        //ジェスチャーをUIViewオブジェクトに登録する
        self.gestureView!.addGestureRecognizer(recognizer)

    }
}
~~~

###ジェスチャーの種類
####UITapGestureRecognizer (タップ)
    
    プロパティ
    numberOfTapsRequired  最小、何本指でタップした時に反応するか
    numberOfTouchesRequired  最大、何本指でタップした時に反応するか

####UISwipeGestureRecognizer (スワイプ)
    numberOfTouchesRequired  最小、何本指で反応するか
    direction   スワイプの方向

####UIPinchGestureRecognizer (ピンチイン＆アウト)
    scale   ピンチイン、アウトの拡大率。ピンチインはマイナスの値、ピンチアウトは正の値
    velocity   加速度

####UILongPressGestureRecognizer (長押し)
長押しを検出する。長押し時のドラッグや長押し終了時にもイベントが発生する

    state : UIGestureRecognizerState  状態
    numberOfTapsRequired   最小何本指のタップで反応するか
    numberOfTouchesRequired  最小何本指のタッチで反応するか(move時)
    minimumPressDuration   長押しの反応時間(デフォルトは 0.5秒)
    allowableMovement   長押し中の指のズレを許容する範囲

####UIPanGestureRecognizer (ピン、ドラッグ)
UIPanGestureRecognizer 
    minimumNumberOfTouches ドラッグ判定される最小の指の数
    maximumNumberOfTouches ドラッグ判定される最大の指の数
    func translationInView ドラッグの移動量を取得。クリアしない場合はドラッグ開始位置からの差分を返す
    func setTranslation  ドラッグ位置をセットする。CGPointZeroでクリアすると毎フレームの移動量が取得できるようになる
    func velocityInView  １秒あたりの移動量(ピクセル)

#セレクタ selector
セレクタは変数に入れられる関数、のようなもの。コンソールのswiftではコンパイルエラーになったので使えないみたいだ。

~~~swift
// Swift2.2以降の書き方
let button = UIButton(type: .System)
button.addTarget(self, action: #selector(ViewController.buttonTapped(_:)), forControlEvents: .TouchUpInside)

func buttonTapped(sender: UIButton) {
    //Button Tapped
}
~~~

#ストレージ storage
<!-- file::　storage::  -->
アプリを終了してもデータを保持できる仕組み

##GitHubサンプルプログラム

|ファイル名|説明|
|---|---|
[UserDefaultViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/StorageTest/StorageTest/UserDefaultViewController.swift) | NSUserDefault
[FileManagerViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/StorageTest/StorageTest/FileManagerViewController.swift) | FileManagerサンプル
[MemoStoreViewController.swift](https://github.com/seafield1979/ios_programing/blob/develop/swift/StorageTest/StorageTest/MemoStoreViewController.swift)  | CoreDataサンプル

##NSUserDefaults
<!-- userdefault:: -->

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

#UIImage
<!-- uiimage:: -->

リンク
[文字列からUIImageオブジェクトを生成する](http://ios.ch3cooh.jp/entry/20140730/1406694860)  


##生成
~~~swift
// プロジェクトに登録済みの画像ファイルを元にUIImageを生成
let image = UIImage(named: "image/ume_r.png")

// 読み込みの成功チェック nil の場合は処理しない
if let imageOK = image {
    imageView1!.image = imageOK
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


#UIColor
<!-- uicolor:: -->
色情報を管理するクラス

~~~swift
// 予め用意された色で生成
let color = UIColor.redColor()

//色の割合を指定して生成
let color = UIColor(red:0.0,green:0.5,blue:1.0,alpha:1.0)

// 画像をパターンとして生成
view.backgroundColor = UIColor(patternImage: UIImage(named: "patternImage.png"))

// "00ff88"のようなHexで色を指定するUIColor拡張
extension UIColor {
    class func hexStr (hexStr : NSString, alpha : CGFloat) -> UIColor {
        let hexStr2 = hexStr.stringByReplacingOccurrencesOfString("#", withString: "")
        let scanner = NSScanner(string: hexStr2 as String)
        var color: UInt32 = 0
        if scanner.scanHexInt(&color) {
            let r = CGFloat((color & 0xFF0000) >> 16) / 255.0
            let g = CGFloat((color & 0x00FF00) >> 8) / 255.0
            let b = CGFloat(color & 0x0000FF) / 255.0
            return UIColor(red:r,green:g,blue:b,alpha:alpha)
        } else {
            print("invalid hex string")
            return UIColor.whiteColor();
        }
    }
}

let color = UIColor.hexStr("FFA73F", alpha:1.0)

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



#座標指定 CGPoint CGRect CGFloat
<!-- cgpoint:: cgrect:: cgfloat:: -->

###CGFloat 浮動小数点

~~~swift
// 変数宣言
var pos_x : CGFloat = 1.0

// Intに変換
let int1 : Int = Int(pos_x)

// CGFloatに変換
pos_x = CGFloat(int1)

// CGFloatとIntで計算ができない
let posX : CGFloat = 10.0
let num : Int = 3
//let posX2 : CGFloat = posX * num  エラー
let posX2 : CGFloat = posX * CGFloat(num)
~~~

###CGPoint 座標
Make系はラベルなし

~~~swift
    // CGPoint を作成する
    let pos = CGPoint(x: 100.0, y: 200.0)
    let pos2 = CGPointMake(100.0, 200.0)

    // viewの座標に設定する
    view1!.frame.origin = CGPoint(x:100.0, y:200.0)
    view2!.frame.origin = pos2
~~~

###CGRect 矩形の座標とサイズ
~~~swift
    // CGRect を作成する
    let rect = CGRect(x:0, y:0, width:100, height:100)
    let rect2 = CGRectMake(0,0,100,100) 

    // viewのフレームに設定する
    view1!.frame = CGRect(x:0, y:0, width: 100, height: 100)
    view2!.frame = rect2
~~~

##layerについて
UIViewはCALayer(.layerプロパティ)を持っている
CALayerはUIViewの上に乗っている
CALayerの座標を変えると、見た目の座標が変わる
UIViewの座標はそのままにCALayerの座標や表示プロパティを変更することで見た目を変えることができる。

#Autolayout

##コードでAutolayoutを追加する
[コードでAutolayout](http://qiita.com/bonegollira/items/5c973206b82f6c4d55ea)

###サンプル
####1つづつ制約を追加する
NSLayoutConstraintオブジェクトを作成し、.addConstraint メソッドで１つづつ制約を追加する

~~~swift
// view1
let view1 = UIView(frame: CGRectMake(50, 50, 100, 50))
view1.backgroundColor = .redColor()

// 先にaddSubviewしないと制約を追加できない
view.addSubview(view1)

view1.translatesAutoresizingMaskIntoConstraints = false

// Top
let topConstraint = NSLayoutConstraint(
    item: view1,
    attribute: .Top,
    relatedBy: .Equal,
    toItem: self.view,
    attribute: .Top,
    multiplier: 1.0,
    constant: 100
)

// Left
let leftConstraint = NSLayoutConstraint(
    item: view1,
    attribute: .Left,
    relatedBy: .Equal,
    toItem: self.view,
    attribute: .Left,
    multiplier: 1.0,
    constant: 10
)

// Width
let widthConstraint = NSLayoutConstraint(
    item: view1,
    attribute: .Width,
    relatedBy: .Equal,
    toItem: self.view,
    attribute: .Width,
    multiplier: 0,
    constant: 100
)

// Heigth
let heightConstraint = NSLayoutConstraint(
    item: view1,
    attribute: .Height,
    relatedBy: .Equal,
    toItem: self.view,
    attribute: .Height,
    multiplier: 0,
    constant: 100
)

view.addConstraint(topConstraint)
view.addConstraint(leftConstraint)
view.addConstraint(widthConstraint)
view.addConstraint(heightConstraint)
~~~

####まとめて制約を追加する
.addConstraintsでまとめて制約を追加する
~~~swift
view1.translatesAutoresizingMaskIntoConstraints = false

// Top
view.addConstraints([
    NSLayoutConstraint(
        item: view1,
        attribute: .Top,
        relatedBy: .Equal,
        toItem: self.view,
        attribute: .Top,
        multiplier: 1.0,
        constant: 100
    ),
    NSLayoutConstraint(
        item: view1,
        attribute: .Left,
        relatedBy: .Equal,
        toItem: self.view,
        attribute: .Left,
        multiplier: 1.0,
        constant: 120
    ),

    NSLayoutConstraint(
        item: view1,
        attribute: .Width,
        relatedBy: .Equal,
        toItem: self.view,
        attribute: .Width,
        multiplier: 0,
        constant: 100
    ),

    NSLayoutConstraint(
        item: view1,
        attribute: .Height,
        relatedBy: .Equal,
        toItem: self.view,
        attribute: .Height,
        multiplier: 0,
        constant: 100
    )]
)
~~~

####いろいろな制約
~~~swift

// Top/Bottom/Left/Right Space
// Viewの上下左右にスペースをもうける
let constraint1 = NSLayoutConstraint(
    item: view1,        // 制約を追加するView
    attribute: .Top,    // .Top / .Bottom / .Left(.Leading) / .Right(.Tailing)
    relatedBy: .Equal,
    toItem: self.view,   // 基準となるView
    attribute: .Top,
    multiplier: 1.0,    // 1.0固定
    constant: 100       // スペースのピクセル数
)

// Width/Height
// 幅、高さを固定値で設定
let constraint2 = NSLayoutConstraint(
    item: view1,    // 制約を追加するView
    attribute: .Width,    // .Width / .Height
    relatedBy: .Equal,
    toItem: self.view,    // 基準となるView
    attribute: .Width,
    multiplier: 0,        // 0: constantの値の幅(高さ) / 0以外: toItemのViewの幅(高さ)の割合(0.5:50%, 1.0: 100%)
    constant: 100         // ピクセル数
)

// 基準Viewの幅、高さの割合(%)を指定
let constraint3 = NSLayoutConstraint(
    item: view1,    // 制約を追加するView
    attribute: .Width,    // .Width / .Height
    relatedBy: .Equal,
    toItem: self.view,    // 基準となるView
    attribute: .Width,
    multiplier: 0.5,        // 基準となるViewの何%の幅(高さにするか)
    constant: 0            // ０固定
)

// センタリング
// 基準位置にViewの中心を合わせる
let centerX = NSLayoutConstraint(
    item: view1,
    attribute: .CenterY,  // 中心(垂直)
    relatedBy: .Equal,
    toItem: self.view,
    attribute: .CenterY,
    multiplier: 1.0,        // センタリング位置 1/2:全体の1/4, 1.0:中央, 3/2:全体の3/4
    constant: 0             // オフセット センタリング位置からどれだけずらすか
)

~~~



#http通信
<!-- http::　https:: -->
httpでサーバーとの通信を行う。  
"http://hoge.com/test/" のようなURLを指定してAPIをコールしレスポンス（応答）を受信する。

##NSURLSession
HTTP通信は以前は NSURLConnection を使っていたが、最新のiOS(iOS7以降)ではNRURLSessionクラスを使用する。

###同期型
同期型は通信が終わるまで処理を待つタイプ。通信が終わるまで処理が帰ってこないので、通信に時間がかかったりするとアプリが固まったようになってしまうため、使用はテスト用程度にしておくのが好ましい。

**同期型サンプル**
~~~swift
func testSynchronism() {
    // create the url-request
    let urlString = "http://sunsunsoft.com/test/ios/hello.php"
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

**php**
~~~php
<?php
    echo "hello world!!\n";
?>
~~~

###非同期型
通信が終わるまで処理を待たないタイプ。受信開始時やデータ受信時、完了時に指定のdelegateメソッドがコールされるので、これらのdelegateメソッドで処理を行う。

**非同期型サンプル**
~~~swift
// プロトコルにNSURLSessionTaskDelegateを追加する
class ViewController: UIViewController, NSURLSessionTaskDelegate {

func testAsynchronous() {
    // 送信したいURLを作成し、Requestを作成します。
    let url = NSURL(string:"http://sunsunsoft.com/test/ios/hello.php")
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

##AFNetworkingのインストール
podからのインストールしたAFNetworkingだとソースが読み込めなかったため、Githubから自前でAFNetworkingのソースを取得しプロジェクトに組み込む。-> [SwiftとAFNetworkingの簡単サンプル 最新情報 2015/01](http://qiita.com/tajihiro/items/294ce7440c8316962da2)  
しかも、最新のバージョン(3.x)だとWebに転がっているようなサンプルが動かないため、古いバージョン(2.x)系を使うようにする。


##リンク

|リンク|説明|
|---|---|
[SwiftとAFNetworkingの簡単サンプル](http://qiita.com/tajihiro/items/ad8606f61f3273423ea8) | GithubからダウンロードしたAFNetworkingをプロジェクトに組み込む方法。※ただし最新のAFNetworkingだとだめ。2.x系を使用すること。
[SwiftとAFNetworkingの簡単サンプル 最新情報 2015/01](http://qiita.com/tajihiro/items/294ce7440c8316962da2) | 上のリンクの方法でAFNetworkingがインストールできない場合に自前でプロジェクトに組み込む方法
[AFNetworking2.xを利用して、HTMLやテキストを取得する](http://qiita.com/hmuronaka/items/7e4f75eae1cccf1007e3) | プレーンテキストを受け取る
[sakaharaのブログ](http://sakahara.hatenablog.jp/entry/2014/06/08/211918) | AFNetworkingを使用したGET,POSTメッセージの送信

##パラメータ
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

#便利・小技 skills

###storyboardを使わないプログラムテンプレート

* プロジェクトの Main.storyboard を削除
* Info.plist内のMain storyboard file base nameの値を空白にする
* General - Deployment Info 内のMain Interfaceの値を空白にする
* AppDelegate.swiftに以下のコードを記述する

~~~swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool
{
    window = UIWindow(frame:UIScreen.mainScreen().bounds)
    
    // 最初に表示されるViewControllerを生成
    let viewController = UIViewController()
    
    // Viewの色を変える
    viewController.view.backgroundColor = UIColor.yellowColor()
    
    window!.rootViewController = viewController
    window!.makeKeyAndVisible();
    return true
}
~~~

###画面サイズを取得
~~~swift
let myBoundSize: CGSize = UIScreen.mainScreen().bounds.size
~~~
