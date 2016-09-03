<!-- Stylesheet -->
<link href="http://sunsunsoft.com/css/hoge.css" rel="stylesheet"></link>

<span class="title">iOSプログラミングのメモ(Owift, Objective-C共通)
</span>

[TOC]

#メモリ管理 strong, weak
<!-- strong:: weak:: -->


1. オブジェクトは強参照されていないと消える
2. オブジェクトを保持する変数はオブジェクトを強参照するか弱参照するかを選べる
3. ローカル変数は強参照
4. addSubviewした場合、親Viewが子Viewを強参照する

オブジェクトを保持する変数は、オブジェクトを強参照(strong)するか弱参照(weak)するかを選べる。
オブジェクトは自分を強参照している変数が１つでもあると生存できる。
強参照している変数が１つもないとその時点で開放される。
ローカルの変数は強参照なので、ローカル変数に代入したオブジェクトはそのスコープを抜けるまでは消えない。（通常スコープを抜ける前にaddSubviewするのでずっと消えない）
UIView系のオブジェクトをaddSubview等で親のViewに登録した場合、親Viewから強参照されるので親Viewが存在し続ける間は生存する。





#AutoLayout
AutoLayoutを使用するとAutoLayoutの制約(Constraints)によって、各Viewの座標やサイズを自動で計算して配置してくれる。自動で配置するには必要な制約をあらかじめ指定しておく必要がある。

##基本
###基本ルール
あるViewが自動で配置されるためには
 
* 座標
* サイズ

がどのような値になるかの情報が必要。この情報を制約(Constraints)で指定してやる。  

####制約を追加する
制約を追加する方法。Interface Builderの右下の方にアイコンがあるのでそこから制約を選んで追加する。

. 制約を追加したいViewを選択する  

![Add Constraints1](http://sunsunsoft.com/image/ios/autolayout/create_constraints1.png)  

. Align アイコンをクリックすると中央揃え系制約のポップアップが表示される  

![Add Constraints2](http://sunsunsoft.com/image/ios/autolayout/create_constraints2.jpg)  

. Pin アイコンをクリックすると座標、サイズ系制約のポップアップが表示される  

![Add Constraints3](http://sunsunsoft.com/image/ios/autolayout/create_constraints3.jpg)  


####指定のViewと同じ幅(高さ)の制約を追加する
指定のViewと同じ幅、高さのWidthを追加する方法は以下。

合わせたい元のViewをCtrlを押しながらドラッグ  
合わせたい先のViewにドロップ  

![Equal Width to](http://sunsunsoft.com/image/ios/autolayout/create_equal_width1.png)  

ポップアップの [Equal Widths] を選択  

![Equal Width to](http://sunsunsoft.com/image/ios/autolayout/create_equal_width2.png)  

Equal Width to: <幅を合わせる先のView名>  
の制約が追加された  
  
![Equal Width to](http://sunsunsoft.com/image/ios/autolayout/create_equal_width3.png)  

Ctrl + ドロップで制約を作成する時に、ドロップ先によってポップアップに表示される制約は異なる

![Equal Width to](http://sunsunsoft.com/image/ios/autolayout/create_equal_width2.png)
![Equal Width to](http://sunsunsoft.com/image/ios/autolayout/create_equal_width21.png)
![Equal Width to](http://sunsunsoft.com/image/ios/autolayout/create_equal_width22.png)  

※左　親Viewにドロップした時のポップアップ  
※真ん中　親以外のViewにドロップした時のポップアップ  
※右　自身のViewにドロップした時のポップアップ  



###サンプル
####例: 幅と座標を指定する場合
左のスペースが100px, 上のスペースが100px, 幅が50px, 高さが50pxのView  

![基本1](http://sunsunsoft.com/image/ios/autolayout/basic1.png)  

  * 左のスペースが100 -> Leading Space = 100  
  * 上のスペースが100 -> Top Space = 100  
  * 幅が50 -> Width = 50  
  * 高さが50 -> Height = 50

![基本1の制約](http://sunsunsoft.com/image/ios/autolayout/basic1_constraints.png)  

####例: 左右(もしくは上下)のスペースから幅(もしくは高さ)が決まる場合
左のスペースが30px, 右のスペースが30px, 上のスペースが40px, 高さが 70px
これは幅が自動で決まるケース  
幅が指定されていないが、親Viewの幅と左右のスペースから幅が自動で計算される
例えば親Viewの幅が 500 で 左右のスペースが 60 だとすると、子Viewの幅は  
500 - 60 = 460  
となる  

![基本2](http://sunsunsoft.com/image/ios/autolayout/basic2.png) 

 * 左のスペースが30 -> Leading Space = 30
 * 右のスペースが30 -> Tailing Space = 30
 * 上のスペースが40 -> Top Space = 40
 * 高さが70 -> Height = 70

![基本2の制約](http://sunsunsoft.com/image/ios/autolayout/basic2_constraints.png) 


###制約(Constrain)の種類
指定できる制約。

|名前|説明|
|!--|!--|
Top Space to <Viewの種類> | 上のスペース
    Bottom Space to <Viewの種類> | 下のスペース
    Leading Space to <Viewの種類> | 左のスペース
    Tailing Space to <Viewの種類>| 右のスペース
Width Equals | 指定の幅
    Height Equals | 指定の高さ
Equal Width to <Viewの種類> | 指定のViewと同じ幅
    Equal Height to <Viewの種類> | 指定のViewと同じ高さ
Proportional Width to <Viewの種類> | 指定Viewの指定の割合(%)の幅
    Proportional Height to <Viewの種類> | 指定Viewの指定割合(%)の高さ
Align Center X to <Viewの種類> | 指定Viewの中心(水平)に合わせる
    Align Center Y to <Viewの種類> | 指定Viewの中心(垂直)に合わせる


##エラーと警告
###エラーが出る場合の対処方法
あるViewに制約を１つでも追加したい状態で、そのViewの座標とサイズを求めるのに必要な制約が足りていない場合や、制約がかぶっている場合などにエラーが発生する。エラーはXCodeにまかせていい具合に直す方法と、自分で直す方法がある。
制約が足りない場合はXCodeにまかせてもOKだが、制約がかぶっている場合はどの制約を残したら良いかXCodeには判断がつかないので自分で直す。

Document Outlineにエラーのアイコンが付く  
  
![制約エラー1](http://sunsunsoft.com/image/ios/autolayout/constraints_error1.png)  
  
<br>
Document Outlineのエラーアイコンをクリックするとエラーの詳細が表示される  
  
![制約エラー2](http://sunsunsoft.com/image/ios/autolayout/constraints_error2.png)  
  
<br>
エラーアイコンをクリックして自動で直すかのポップアップが表示された例。この場合は足りていない制約を追加するかどうか聞かれている。
  
![制約エラー3](http://sunsunsoft.com/image/ios/autolayout/constraints_error3.png)  
<br>

警告をクリックして解決の方法を選択するポップアップが表示された例。

![制約エラー4](http://sunsunsoft.com/image/ios/autolayout/constraints_error4.png)  
<br>

###エラーと警告の種類
Todo


##応用
###画面の80%の幅を持つView
親の幅、高さの何パーセントかを指定するConstraint 

完成系のイメージ  
画面の80%の幅。中心にセンタリング。高さ。トップスペース。  

![パーセント指定1](http://sunsunsoft.com/image/ios/autolayout/percent_width1.png)  

水平座標を中心に配置  
[Align] - [Holizontally in Container] をチェック  
![パーセント指定2](http://sunsunsoft.com/image/ios/autolayout/percent_width2.png)  
  
Document Outlineで  
親のView＆幅を合わせたいViewを選択  

![パーセント指定3](http://sunsunsoft.com/image/ios/autolayout/percent_width3.png)  
  
[Pin] - [Equal Widths] をチェック  
これで親Viewと子Viewの幅が同じになる。  

![パーセント指定4](http://sunsunsoft.com/image/ios/autolayout/percent_width4.png)  
  
子Viewを選択  
size inspector を選択  
[Constraints] - [Equal Width to~] の Edit をクリック  
![パーセント指定5](http://sunsunsoft.com/image/ios/autolayout/percent_width5.png)  
  
[Equal Width Constraints]の詳細から  
[Multiplier] の値を設定する(親の80%の幅にしたい場合は0.8)  
  
![パーセント指定6](http://sunsunsoft.com/image/ios/autolayout/percent_width6.png)  

足りないConstraintsを追加
Top Space, Height
  
![パーセント指定7](http://sunsunsoft.com/image/ios/autolayout/percent_width7.png)  

プログラムを実行  
子Viewが親Viewの80%の幅になっている  
  
![パーセント指定8](http://sunsunsoft.com/image/ios/autolayout/percent_width8.png)  



###複数のViewを等間隔に並べる
同じ幅のViewを２つ、横に等間隔に並べる。

こんな感じ↓  
![等間隔配置](http://sunsunsoft.com/image/ios/autolayout/equal_interval.png)  


最終的なConstraintsは以下
View1(左側)

* Align Center X to: Superview (Multiplier = 1/2)<br>
    中心のxは親Viewの1/2(実際には1/4)
* Proportional Width to: Superview (Multiplier = 0.4)<br>
    Widthは親のWidthの 0.4倍
* Top Space to Superview 27
* Height 128

View2(右側)

* Align Center X to: Superview (Multiplier = 1/2)<br>
    中心のxは親Viewの3/2(実際には3/4)
* Proportional Width to: Superview (Multiplier = 0.4)<br>
    Widthは親のWidthの 0.4倍
* Top Space to Superview 27
* Height 128

<br><br>
View1(左側)のConstraints  ↓

![等間隔配置1](http://sunsunsoft.com/image/ios/autolayout/equal_interval1.png)  

Align Center ~ の Multiplier は 1/2  
これでViewの中心は 1/4 あたりに来る  ↓

![等間隔配置2](http://sunsunsoft.com/image/ios/autolayout/equal_interval2.png)  

Proportional Width ~ の Multiplier は 0.4  
これでViewのWidthは親のWidthの0.4倍　　↓

![等間隔配置3](http://sunsunsoft.com/image/ios/autolayout/equal_interval3.png)  

View２(右側)のConstraints  ↓

![等間隔配置4](http://sunsunsoft.com/image/ios/autolayout/equal_interval4.png)  

Align Center ~ の Multiplier は 3/2  
これでViewの中心は親の 3/4 あたりに来る  ↓

![等間隔配置5](http://sunsunsoft.com/image/ios/autolayout/equal_interval5.png)  

Proportional Width ~ の Multiplier は 0.4  
これでViewのWidthは親のWidthの0.4倍　　↓

![等間隔配置6](http://sunsunsoft.com/image/ios/autolayout/equal_interval6.png)  
