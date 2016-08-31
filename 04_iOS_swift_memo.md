<!-- Stylesheet -->
<link href="http://kevinburke.bitbucket.org/markdowncss/markdown.css" rel="stylesheet"></link>
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">SwiftでiOSアプリ作成</span>

<!-- もくじ -->
[TOC]

#リンク
[日本語に翻訳されたiOS/watchOS/tvOSのドキュメント](https://developer.apple.com/jp/documentation/)

$x^2 + y^2 = 1$  
$a = \{1, 2, 3\}$  
$a = \\{1, 2, 3\\}$  

#プログラム作成
###プロジェクトを作ったやら最初にすること(storyboardを使わない方法)

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

#UIWindow
###Windowを取得する
~~~swift
window = UIWindow(frame:UIScreen.mainScreen().bounds)
~~~

###

#UIViewController
###画面に表示する
windowのトップにViewControllerを表示する  

~~~swift
// UIViewControllerを生成して画面に貼り付ける(※ただしUIViewの色を変えないと見た目の変化なし)
let viewController : UIViewController = UIViewController()
window!.rootViewController = viewController
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

#UIView(とそのSubClass)
<!-- uiview:: -->
矩形を表示するオブジェクト。とりあえず画面に何か表示したいときはこのクラスか、このクラスのサブクラスを作ってメインのViewにaddSubviewする。

##UIView
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

##UIResponderの機能
タッチやシェイク等のユーザー操作系のイベントを処理するためのクラス

UIViewもUIResponderクラスのサブクラス。なのでUIViewでもタッチ等のイベントが取れる。

##UILabel
画面にテキストを表示したい場合に使用する

###生成&表示
~~~swift
let label1 : UILabel = UILabel(frame: CGRect(x:50, y:100, width:100, height: 50))
label1.text = "Hello World!!"
viewController.view.addSubview(label1)
~~~

###フォントを変更
<!-- font:: -->

###テキストの色を変更
<!-- textcolor: -->

##UIButton
<!-- button:: -->
###リンク
ハイライト時のBG色を設定する方法。直接色を指定できないのでUIColorからUIImageを作成して設定する。  
[UIButtonハイライト時の色を指定する](http://qiita.com/akatsuki174/items/c0b8b5126b6c12f62001)

###ボタン生成のサンプル
~~~swift
func createSimpleButton() {
    let button = UIButton(frame: CGRectMake(pos.x, pos.y, 100, 30))
    button.setTitle(title, forState: UIControlState.Normal)
    button.setTitleColor( UIColor.grayColor(), forState: UIControlState.Highlighted)
    // 押された時の処理

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

###ステータスバーを非表示にする
UIViewControllerクラスに以下のメソッドを追加
~~~swift
override func prefersStatusBarHidden() -> Bool {
    return true
}
~~~

#ジェスチャー
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

#UIImage
##生成
~~~swift

~~~

#座標指定 CGPoint CGRect CGFloat

##CGFloat 浮動小数点

~~~swift
// 変数宣言
var pos_x : CGFloat = 1.0

// Intに変換
let int1 : Int = Int(pos_x)

// CGFloatに変換
pos_x = CGFloat(int1)
~~~

##CGPoint 座標
Make系はラベルなし

~~~swift
    // CGPoint を作成する
    let pos = CGPoint(x: 100.0, y: 200.0)
    let pos2 = CGPointMake(100.0, 200.0)

    // viewの座標に設定する
    view1!.frame.origin = CGPoint(x:100.0, y:200.0)
    view2!.frame.origin = pos2
~~~

##CGRect 矩形の座標とサイズ
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

#注釈
